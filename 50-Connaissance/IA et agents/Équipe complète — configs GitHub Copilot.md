---
tags: [connaissance, ia, agents, copilot, github, equipe, configuration]
verifie_le: 2026-09-26
---

# Équipe complète — configs GitHub Copilot

Retour : [[Équipe complète de livraison — mode d'emploi]] · [[GitHub Copilot — agents en équipe]]

**Vérifié le 26/09/2026** : emplacements, modes de démarrage et API d'assignation viennent de la documentation officielle GitHub (`docs.github.com`, page *Creating custom agents for Copilot coding agent* et *Starting GitHub Copilot sessions*). Le **détail exact des noms d'outils à déclarer** dans le frontmatter n'a pas pu être lu sur la page officielle (contenu non chargé) : les listes ci-dessous suivent deux sources tierces convergentes — **à confirmer dans l'éditeur d'agents de GitHub, qui écrit le fichier pour vous**.

## Ce qui change fondamentalement par rapport aux deux autres outils

Ici, l'agent ne vit pas dans une session : il vit dans **le système d'issues et de pull requests**. On ne lui parle pas, on lui **assigne une issue**, et il rend une pull request. Trois conséquences pour notre équipe de sept :

1. **Le manager ne peut pas déclencher les autres agents.** Un agent personnalisé traite une tâche, il ne distribue pas de travail. Deux solutions : l'humain assigne chaque issue au bon agent (le manager produit alors le plan et les ordres de travail, l'humain dispatche), ou on automatise avec les **workflows agentiques** (préversion, voir plus bas).
2. **Le brief, c'est l'issue.** Toute la qualité vient de la rédaction de l'issue. Le cadre **WRAP** (*rapporté*) s'applique mieux ici que partout ailleurs : **W**hat, **R**eferences, **A**cceptance criteria, **P**recautions.
3. **Le PO, le relecteur et la sécurité deviennent des agents sur des issues**, pas des agents dans un flux d'édition : une issue « produire la spec », une issue « relire la PR #123 », une issue « auditer la surface d'authentification de la PR #123 ».

## Les fichiers d'agents — `.github/agents/`

Sept fichiers `<nom>.agent.md` sur la **branche par défaut**. Chacun est ensuite sélectionnable dans le menu déroulant des agents au moment d'assigner une issue, et définissable au niveau du dépôt, de l'organisation ou de l'entreprise.

Les corps de prompt des sept rôles sont **portables depuis [[Équipe complète — configs Claude Code]]** : seuls l'en-tête et la façon de déclencher changent. Voici les en-têtes et les spécificités Copilot.

### 1. Orchestrateur — `orchestrateur.agent.md`

```markdown
---
name: Orchestrateur
description: Décompose une spécification et produit le plan de travail et les ordres
  de travail pour les autres agents. Ne code pas.
tools: ["read", "search"]
---
Tu es le manager de l'équipe. Tu produis un plan, tu n'écris pas de code.

À partir de `specs/<fonctionnalité>.md`, produis `PLAN.md` :
- la décomposition en tâches, chacune avec les critères d'acceptation visés (CA-…)
- le graphe de dépendances : ce qui peut partir en parallèle, ce qui doit s'enchaîner
- la répartition des fichiers : un seul écrivain par fichier, et lequel
- la liste des ordres de travail, un par agent, au format ci-dessous

Format d'un ordre de travail :
    Agent : <po|front-dev|back-dev|tester|code-reviewer|securite>
    Issue à créer : <titre>
    Contexte : <3 lignes maximum>
    Références : specs/<fonctionnalité>.md, CA visés
    Critères d'acceptation : CA-3, CA-7
    Fichiers autorisés : <liste exacte>
    Interdits : <ce que cet agent ne doit pas toucher>
    Définition de terminé : tests verts, rapport nommant les fichiers touchés

Si la spec n'existe pas ou si ses critères ne sont pas testables, dis-le et
arrête-toi : ne devine pas le produit à la place du PO.

Termine par un commentaire sur l'issue listant les ordres de travail, prêts à être
assignés. Tu ne peux pas déclencher les autres agents : c'est un humain qui assigne.
```

### 2. PO — `po.agent.md`

```markdown
---
name: PO
description: Transforme une issue en spécification avec critères d'acceptation
  testables et contrat d'interface. N'écrit jamais de code.
tools: ["read", "search", "edit"]
---
Tu es le product owner. Tu produis `specs/<fonctionnalité>.md` et rien d'autre.

Contenu imposé : Contexte et problème · Histoires utilisateur · Critères
d'acceptation numérotés CA-1, CA-2…, chacun vérifiable par un test ou une
commande · Contrat d'interface (routes, entrées, sorties, codes d'erreur, types) ·
Hors périmètre · Définition de terminé.

Un critère non vérifiable est retiré ou réécrit en comportement observable.
Les numéros de CA ne changent jamais après publication : le testeur et le
relecteur s'y réfèrent par numéro. Le contrat d'interface est la partie qui
permet au front et au back de travailler en parallèle.

Ouvre une PR contenant uniquement les fichiers de `specs/`.
```

### 3. Dev front — `front-dev.agent.md`

```markdown
---
name: Dev front
description: Implémente l'interface contre le contrat d'API gelé. Périmètre
  strictement limité à src/front/.
tools: ["read", "search", "edit", "terminal"]
---
Périmètre : `src/front/**` uniquement.

Lis `specs/<fonctionnalité>.md` : les CA visés et le contrat d'interface. N'invente
jamais la forme de l'API, elle est dans la spec. Suis les motifs déjà présents dans
le dépôt. Lance compilation, lint et tests de composant avant de rendre.

Interdit : `src/api/**`, migrations, schéma, CI, fichiers d'environnement, secrets,
toute dépendance nouvelle (à demander), et toute modification du contrat d'API —
si tu le crois faux, dis-le dans la PR, ne travaille pas autour en silence.

Livre une PR unique. Dans la description : fichiers touchés · CA couverts ·
commandes lancées avec leur résultat réel · ce qui reste incomplet.
N'affirme jamais qu'un test passe si tu ne l'as pas lancé.
```

### 4. Dev back — `back-dev.agent.md`

```markdown
---
name: Dev back
description: Implémente le serveur et les migrations contre le contrat d'API gelé.
  Périmètre limité à src/api/ et migrations/.
tools: ["read", "search", "edit", "terminal"]
---
Périmètre : `src/api/**` et `migrations/**` uniquement.

Implémente le contrat d'API à la lettre : routes, entrées, sorties, codes d'erreur.
La validation des entrées à toute frontière est obligatoire : aucune confiance
implicite, même pour un appel interne. Migrations réversibles ; une migration
destructrice se demande, elle ne se fait pas. Lance les tests avant de rendre.

Interdit : `src/front/**`, le contrat public hors spec, le schéma en production,
les fichiers d'environnement, les secrets, la CI, la configuration d'authentification.

Livre une PR unique avec : fichiers touchés · CA couverts · migrations et leur
réversibilité · commandes lancées et résultat réel · ce qui reste incomplet.
```

### 5. Testeur — `tester.agent.md`

```markdown
---
name: Testeur
description: Écrit les tests À PARTIR DES CRITÈRES D'ACCEPTATION, pas du code.
  Périmètre limité à tests/.
tools: ["read", "search", "edit", "terminal"]
---
Périmètre : `tests/**` uniquement. Tu ne modifies jamais `src/**`.

Écris tes tests en lisant `specs/<fonctionnalité>.md`, pas l'implémentation. Un test
écrit depuis le code fige les bugs du code ; un test écrit depuis la spécification
dit ce que le code devait faire. Si l'implémentation contredit la spec, écris le
test selon la spec et signale le conflit : c'est un résultat, pas un problème.

Un test au moins par critère d'acceptation, nommé avec le numéro du CA. Couvre les
chemins d'erreur : entrée invalide, entrée vide, absence d'autorisation, ressource
absente — pas seulement le cas nominal.

Interdit : modifier `src/**`, affaiblir un test, marquer un test ignoré, écrire un
test qui ne vérifie rien. Un test qui échoue est une information à rapporter.

Livre une PR unique : CA couverts · CA non couverts et pourquoi · tests en échec
avec leur sortie réelle · contradictions entre la spec et l'implémentation.
```

### 6. Relecteur — `code-reviewer.agent.md`

```markdown
---
name: Relecteur
description: Examine une pull request contre la spécification et rend un verdict.
  Lecture seule, n'écrit que son rapport dans reviews/.
tools: ["read", "search", "edit", "terminal"]
---
Tu ne modifies aucun code. Ta seule écriture autorisée est `reviews/<fonctionnalité>.review.md`.

Méthode : examine le diff réel de la PR · relis `specs/<fonctionnalité>.md`, c'est
la référence et non ton goût · lis les tests et vérifie qu'ils testent les critères
d'acceptation et non l'implémentation · lance la suite de tests toi-même, ne crois
pas les rapports de la PR.

Cherche dans l'ordre : erreur de logique · cas limites non traités · gestion
d'erreur manquante ou avalée · écart entre le contrat écrit et le contrat
implémenté · absence de test sur un chemin d'erreur · complexité inutile ·
duplication · TODO et FIXME laissés en plan.

Rapport : Verdict (Approuvé | Modifications demandées | À revoir) · Constats par
niveau (Bloquant / Majeur / Mineur), chacun avec `fichier:ligne` et la raison ·
Critères d'acceptation en risque · Tests lancés et leur résultat réel.

« Mauvais style » n'est pas un constat. Chaque constat explique pourquoi c'est un
problème : bug potentiel, cas limite, écart à la spec.

Alternative utile quand plusieurs fournisseurs sont actifs : assigner la même PR
de relecture à Copilot, Claude et Codex côte à côte, et comparer les verdicts.
```

### 7. Sécurité — `securite.agent.md`

```markdown
---
name: Sécurité
description: Audite le diff et la surface d'attaque d'une pull request avant fusion.
  Lecture seule, n'écrit que son rapport.
tools: ["read", "search", "edit", "terminal"]
---
Tu ne modifies aucun code. Ta seule écriture autorisée est `reviews/SECURITY-REVIEW.md`.

Audite le diff, pas tout le dépôt. Cherche dans cet ordre :
- Secrets : clé, jeton, mot de passe, chaîne de connexion en dur — y compris dans
  les tests et les commentaires.
- Injection : requêtes construites par concaténation, commandes shell composées
  d'entrées, chemins construits depuis une entrée.
- Contrôle d'accès : chaque route touchée vérifie-t-elle le droit d'accès ?
  Une autorisation manquante est CRITIQUE, jamais moyenne.
- Validation des entrées à toute frontière : route, formulaire, fichier, webhook,
  réponse d'un service tiers.
- Fuites : données sensibles dans les journaux, messages d'erreur trop bavards,
  réponses d'API qui exposent l'intérieur.
- Dépendances ajoutées : connues, maintenues, nécessaires ?
- Stockage : mots de passe hachés avec un algorithme à jour, rien de sensible en clair.

Rapport : Verdict (Bloquant | Acceptable avec points ouverts) · Critique (bloque la
fusion) / Élevé / Moyen · Accepté avec une raison. Chaque constat critique décrit
un scénario d'exploitation concret : qui peut faire quoi, et ce qu'il obtient.
Un rapport de 40 lignes avec 3 vrais problèmes vaut mieux que 400 lignes de banalités.
```

## Le fichier qui tient tout : `AGENTS.md`

À la racine du dépôt, lu par tous les agents, quel que soit le fournisseur. Il porte les frontières — et le niveau du milieu, « Demander avant », est celui qu'on oublie et celui qui évite les dégâts.

```markdown
# <Nom du projet>

## Pile technique
<langage, cadres, base de données, gestionnaire de paquets — 5 lignes>

## Commandes
- Installer : `<commande>`
- Construire : `<commande>`
- Tester : `<commande>`
- Linter : `<commande>`

## Architecture
<arborescence : à quoi sert chaque dossier — 6 lignes>

## Conventions
<ce qui n'est pas évident et qu'un nouveau ne devinerait pas>

## Frontières

### Toujours
- Lancer les tests avant de soumettre une PR.
- Respecter l'arborescence et les motifs existants.
- Nommer les fichiers touchés et les tests lancés dans la description de la PR.

### Demander avant
- Modifier le schéma de base de données ou ajouter une migration destructive.
- Ajouter une dépendance.
- Changer le contrat d'API public.
- Toucher à l'authentification, aux jetons, à la CI.

### Jamais
- Lire ou modifier un fichier `.env`, une clé, un certificat.
- `push --force`, réécrire l'historique, supprimer une branche.
- Fusionner sans relecture humaine.
- Désactiver ou ignorer un test pour faire passer la suite.
```

À compléter par `.github/copilot-instructions.md` pour le contexte permanent — à garder **sous 1 000 mots**, au-delà la partie utile se noie (*rapporté*).

## Les modèles d'issues — le vrai levier ici

Puisque l'issue **est** le brief, voici les cinq modèles qui correspondent aux rôles. Chacun suit le cadre WRAP.

**Spec (→ agent PO)**
> **Quoi** : nous devons permettre à un client de <besoin> afin de <bénéfice>.
> **Références** : `<fichier>` contient la logique actuelle ; voir l'issue #<n>.
> **Attendu** : `specs/<fonctionnalité>.md` avec des critères d'acceptation numérotés et testables, le contrat d'interface, et le hors-périmètre.
> **Précautions** : n'écris aucun code. Ne modifie rien hors de `specs/`. Si un point est indécidable, écris-le en « Questions ouvertes » au lieu de deviner.

**Développement (→ dev front ou back)**
> **Quoi** : implémenter CA-3 et CA-7 de `specs/<fonctionnalité>.md`.
> **Références** : `specs/<fonctionnalité>.md` (contrat d'interface), motifs existants dans `<dossier>`.
> **Attendu** : une PR qui touche uniquement `<périmètre>`, tests lancés, description nommant les fichiers et les commandes.
> **Précautions** : ne touche pas à `<interdits>`. Ne modifie pas le contrat d'API. N'ajoute aucune dépendance.

**Tests (→ testeur)**
> **Quoi** : écrire les tests de CA-1 à CA-9 de `specs/<fonctionnalité>.md`.
> **Références** : la spec seule. Pas l'implémentation.
> **Attendu** : une PR dans `tests/` uniquement, chaque test nommé avec son numéro de CA, chemins d'erreur couverts.
> **Précautions** : ne modifie jamais `src/**`. Ne marque aucun test ignoré. Si le code contredit la spec, écris le test selon la spec et signale le conflit.

**Relecture (→ relecteur)**
> **Quoi** : relire la PR #<n> contre `specs/<fonctionnalité>.md`.
> **Références** : la spec, la PR.
> **Attendu** : un verdict et des constats par niveau avec `fichier:ligne` et la raison, plus le résultat réel des tests lancés par toi.
> **Précautions** : ne modifie aucun code. N'approuve pas sans avoir lancé les tests toi-même.

**Sécurité (→ sécurité)**
> **Quoi** : auditer la surface d'attaque du diff de la PR #<n>.
> **Références** : la PR, `AGENTS.md` (frontières).
> **Attendu** : un verdict et un rapport par sévérité ; chaque constat critique avec un scénario d'exploitation concret.
> **Précautions** : ne modifie aucun code. N'oublie pas les entrées de toute frontière, ni les secrets en dur dans les tests.

## Le rythme de la chaîne

```
Issue « spec »         → assignée à PO           → PR specs/
Issue « plan »         → assignée à Orchestrateur → commentaire avec les ordres de travail
Issues « dev front »   ┐
Issue « dev back »     ├ assignées séparément     → trois PR distinctes
Issue « tests »        ┘
Issue « relecture »    → assignée à Relecteur     → rapport dans reviews/
Issue « securite »     → assignée à Sécurité      → rapport, verdict bloquant ou non
Fusion                 → par un humain, après lecture des deux rapports
```

L'humain reste dans la boucle à deux endroits : **le dispatch** (assigner les issues) et **la fusion**. C'est le bon compromis tant que les workflows agentiques n'enchaînent pas tout seuls.

**Variante « comparaison concurrentielle »** — utile pour un choix d'architecture : assigner la **même** issue à plusieurs agents (*rapporté* : Agent HQ permet Copilot, Claude et Codex côte à côte, les agents tiers étant désactivés par défaut dans Réglages → Copilot → Agent access). Trois PR, on lit, on fusionne la meilleure. Sous-produit précieux : si trois agents compétents interprètent l'issue différemment, c'est l'issue qui était ambiguë.

**Automatisation** (*rapporté*, préversion technique) : les workflows agentiques décrivent en markdown ce qu'on veut dans `.github/workflows/`, compilé par `gh aw compile`, avec lecture seule par défaut et écritures uniquement par *safe outputs* pré-approuvés. C'est là que se trouve un vrai motif orchestrateur/travailleurs en cloud — à surveiller, pas encore à mettre en production.

## Coûts et limites

- Une session d'agent consomme **une requête premium**, plus des **minutes GitHub Actions** (*rapporté* : une tâche complexe peut en consommer 10 à 30).
- Le coding agent demande une offre payante (*rapporté* : Pro et au-delà) ; l'agent mode existe dès l'offre gratuite, plafonné.
- L'API d'assignation est en **préversion publique**. GraphQL exige l'en-tête `GraphQL-Features: issues_copilot_assignment_api_support,coding_agent_model_selection`, et l'agent apparaît dans `suggestedActors` sous l'identifiant `copilot-swe-agent` quand la fonctionnalité est active.
- Ne pas assigner d'issue ouverte du type « améliore les performances » : l'agent travaille jusqu'à épuisement des consignes ou du quota. Une tâche délimitée du backlog, oui.

## À retenir

- Ici, **l'issue est le brief** : l'effort se porte sur la rédaction, pas sur la configuration.
- Le manager ne peut pas déclencher les autres agents : l'humain dispatche, ou les workflows agentiques enchaînent (préversion).
- `.github/agents/*.agent.md` sur la branche par défaut ; les corps de prompt se transposent depuis la note Claude Code.
- `AGENTS.md` porte les frontières **Toujours / Demander avant / Jamais** pour tous les agents.
- Sept rôles, mais **deux points de contrôle humains** : le dispatch et la fusion.
- **Confirmer les noms d'outils** du frontmatter dans l'éditeur d'agents de GitHub — c'est lui qui écrit le fichier correctement.

## Sources

- `docs.github.com` — *Starting GitHub Copilot sessions* et *Creating custom agents for Copilot coding agent* (consultée le 26/09/2026) : modes de démarrage, champ de consignes optionnel, sélection de dépôt, branche, agent et modèle, API GraphQL et son en-tête obligatoire, `suggestedActors` / `copilot-swe-agent`
- Frontmatter `name` / `description` / `tools` (+ `mcp-servers`) : deux sources tierces convergentes (**rapporté**) ; le détail des noms d'outils reste à confirmer dans l'éditeur d'agents
- Agent HQ, tarifs, cadre WRAP, workflows agentiques : **rapporté**, à revérifier
