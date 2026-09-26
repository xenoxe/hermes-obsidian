---
tags: [connaissance, ia, agents, configuration, agents-md]
verifie_le: 2026-09-26
---

# AGENTS.md — le contrat commun aux agents

Retour : [[IA et agents — Index]] · Utilisé par : [[Équipe complète de livraison — mode d'emploi]] · [[Équipe complète — configs Claude Code]] · [[Équipe complète — configs Cursor]] · [[Équipe complète — configs GitHub Copilot]]

**Rédigé le 26/09/2026.** Ce que c'est : un fichier markdown à la racine d'un dépôt qui décrit le projet et **les frontières** aux agents. Lu par tous les agents, quel que soit le fournisseur — c'est la généralisation de `CLAUDE.md` à tout l'écosystème.

## Les faits, et les deux réserves

Ce qui est **vérifié** : Cursor documente `AGENTS.md` à côté de ses règles de projet (`cursor.com/docs/context/rules`), et Copilot place `AGENTS.md` dans ses fichiers d'instructions, le coding agent lisant aussi `CLAUDE.md` et `GEMINI.md` s'ils existent.

Ce qui est **rapporté** par des sources tierces, et qu'il faut donc tenir pour probable plutôt que certain : GitHub aurait analysé plus de 2 500 `AGENTS.md` publics ; on peut en placer dans les sous-dossiers d'un monorepo pour donner des consignes différentes selon la zone du dépôt ; `.github/copilot-instructions.md` serait le contexte permanent, à garder sous 1 000 mots.

**Réserve 1 — ce n'est pas une norme, c'est une convention.** Aucun organisme ne la contrôle, le support varie selon les outils et les versions. Conséquence pratique : garder `AGENTS.md` comme source principale et, si un outil l'ignore ou le lit partiellement, ajouter son fichier propre (`CLAUDE.md`, `.cursor/rules/`) qui **pointe** vers lui plutôt que de recopier son contenu. Deux copies divergentes valent moins que zéro.

**Réserve 2 — un `AGENTS.md` trop long ne sert à rien.** La documentation Cursor le dit explicitement pour les règles : ne pas recopier un guide de style entier (le linter s'en charge), ne pas documenter chaque commande (l'agent connaît `npm`, `git`, `pytest`), ne pas écrire de règles pour des cas limites rares. Chaque ligne doit être **porteuse** : si l'agent le devinait tout seul, elle coûte sans rien apporter.

## Ce que chaque section apporte

| Section | Ce qu'elle empêche |
|---|---|
| **Vue d'ensemble** — pile, versions, points d'entrée | Que l'agent devine la pile et propose la mauvaise bibliothèque |
| **Commandes** — installer, construire, tester, linter, **et la commande pour lancer UN seul test** | Que l'agent invente une commande, échoue, et brûle dix tours à chercher. C'est la section au meilleur rendement de tout le fichier |
| **Architecture** — arborescence, rôle de chaque dossier | Qu'il crée une structure parallèle à la tienne |
| **Conventions** — seulement ce qui n'est pas devinable | Qu'il introduise un style étranger au dépôt |
| **Tests** — cadre, base de test, nommage, « couvre les chemins d'erreur » | Des tests qui ne testent que le cas nominal |
| **Frontières** en trois niveaux | Les dégâts : c'est la section décisive |
| **Sécurité** — jamais de secret lu, écrit ou recopié | Une clé dans un commit |
| **Définition de terminé** et format du rapport | Les « c'est fait » non vérifiables |

## Les trois niveaux de frontière, et pourquoi le milieu compte

- **Toujours** — ce qui va de soi une fois dit : lancer les tests avant de soumettre, respecter l'arborescence, committer sur sa branche.
- **Demander avant** — migration ou changement de schéma, ajout de dépendance, modification du contrat d'API public, authentification, jetons, CI.
- **Jamais** — lire ou modifier un `.env` ou un secret, `push --force`, supprimer une branche, fusionner sans relecture humaine, désactiver un test.

Le niveau **« Demander avant »** est celui qu'on oublie partout, et c'est le seul qui gère le cas gênant : l'agent **compétent** qui sort du cadre. Un « toujours » ne l'arrête pas, un « jamais » ne le concerne pas — il fait quelque chose de raisonnable que tu n'avais pas prévu. C'est là que se prennent les décisions coûteuses à défaire.

## Le modèle, à copier

```markdown
# <Nom du projet>

## Vue d'ensemble
<Ce que fait le projet, en 2 lignes.>
Pile : <langage + version, cadre, base de données, gestionnaire de paquets>.
Points d'entrée : <fichier principal, point de montage de l'API, dossier des pages>.

## Commandes
- Installer : `<commande>`
- Développer : `<commande>`
- Construire : `<commande>`
- Tester tout : `<commande>`
- Tester UN seul fichier : `<commande> <chemin>`   ← à remplir, c'est la plus utilisée
- Linter : `<commande>`
- Vérifier les types : `<commande>`

## Architecture
- `<dossier>/` — <rôle, 5 mots>
- `<dossier>/` — <rôle>
- Ne pas créer de nouveau dossier de premier niveau sans le demander.

## Conventions
- <export nommé plutôt qu'export par défaut, par exemple>
- <comment les erreurs sont gérées : exceptions typées, jamais de `null` en cas d'échec>
- <nommage des fichiers et des fonctions>
- <langue des identifiants, langue des textes affichés, format des dates>
- Ne pas reformater du code non touché.

## Tests
- Cadre : <cadre>. Base de test : <comment elle est obtenue>.
- Un test par comportement, nommé par le comportement attendu.
- Couvrir les chemins d'erreur : entrée invalide, entrée vide, absence d'autorisation,
  ressource absente — pas seulement le cas nominal.
- Les tests existants doivent continuer de passer.

## Frontières

### Toujours
- Lancer les tests avant de soumettre.
- Respecter l'arborescence et les motifs existants.
- Committer sur sa propre branche.
- Nommer dans le rapport : fichiers touchés, commandes lancées, résultat réel.

### Demander avant
- Modifier le schéma de base de données ou ajouter une migration destructive.
- Ajouter une dépendance, même petite, même « évidente ».
- Modifier le contrat d'API public.
- Toucher à l'authentification, aux jetons, à la CI, aux fichiers de configuration
  partagés.

### Jamais
- Lire, écrire ou recopier un `.env`, une clé, un jeton, un certificat.
- `git push --force`, réécrire l'historique, supprimer une branche.
- Fusionner sans relecture humaine.
- Désactiver ou ignorer un test pour faire passer la suite.

## Sécurité
- Les secrets vivent dans l'environnement, jamais dans le dépôt. Les nommer par
  leur variable (`API_KEY`), jamais par leur valeur.
- Toute entrée venue de l'extérieur est validée à la frontière : route, formulaire,
  fichier, webhook, réponse d'un service tiers.
- Un message d'erreur ne révèle pas l'intérieur du système.

## Définition de terminé
Une tâche est terminée quand : les tests passent (lancés par celui qui l'affirme) ·
le linter et la vérification de types passent · les critères d'acceptation visés
sont couverts par un test · aucune frontière n'a été franchie · le rapport final
nomme les fichiers touchés et les commandes lancées.

## Format du rapport final
Fichiers touchés · critères d'acceptation couverts · commandes lancées et leur
résultat réel · ce qui reste incomplet · toute divergence avec la spécification.
Ne jamais affirmer qu'un test passe sans l'avoir lancé.
```

## Exemple rempli, pour voir le niveau de précision attendu

Extrait d'un projet Node/TypeScript — **exemple, à remplacer par ta pile** :

```markdown
## Commandes
- Installer : `pnpm install`
- Développer : `pnpm dev`
- Tester tout : `pnpm test`
- Tester UN seul fichier : `pnpm vitest run <chemin>`
- Linter : `pnpm lint` (et `pnpm lint --fix` pour corriger)
- Vérifier les types : `pnpm tsc --noEmit`

## Conventions
- Exports nommés uniquement. Pas d'export par défaut.
- Toute erreur attendue est une exception typée ; on ne renvoie jamais `null`
  pour signaler un échec.
- Validation des entrées par schéma à la frontière, jamais au fond de la logique.
- Identifiants en anglais, textes affichés en français, dates en ISO 8601 stockées,
  affichées en `jj/mm/aaaa`.
- Ne pas reformater un fichier qu'on n'a pas modifié.
```

La différence avec le squelette tient en deux choses : **la commande de test unitaire** (celle qu'un agent cherchera sinon pendant plusieurs minutes), et les conventions qui ne se devinent pas (exports nommés, exceptions typées, langue des identifiants).

## Une instance complète, prête à copier

Le modèle ci-dessus existe aussi **rempli, en arborescence complète**, dans `90-Templates/instance-nestjs-fullstack/` — pour la pile réelle de Moh Amed (NestJS pour l'API, Vue ou React pour l'interface, PostgreSQL partout). Ce n'est plus un fichier isolé mais un dossier à copier tel quel à la racine d'un dépôt :

- `AGENTS.md` et `CLAUDE.md` — le contrat et son renvoi
- `.claude/agents/` — sept agents avec leur frontmatter complet (`tools`, `model`, `permissionMode`, `maxTurns`, `isolation: worktree`)
- `.cursor/agents/` + `.cursor/rules/` — six agents et **six règles** (doctrine du manager, frontières, API NestJS, base de données, interface, tests)
- `.github/agents/`, `.github/copilot-instructions.md`, `.github/ISSUE_TEMPLATE/` — sept agents Copilot et cinq modèles d'issues
- `specs/`, `reviews/` — les dossiers de travail où circulent les artefacts entre agents

Les corps de prompt des agents sont **générés depuis une source unique** : ils ne peuvent pas diverger d'un outil à l'autre. Une notice (`README — comment copier cette instance.md`) explique la copie, ce qu'il reste à remplir, et le point structurel du manager — un sous-agent ne pouvant pas en engendrer d'autres, il est la session principale sous Claude Code, une règle sous Cursor, et un agent qui produit des ordres de travail sous Copilot.

Ce que l'instance ajoute au modèle générique, parce que c'est là qu'un SaaS se casse réellement :

- **PostgreSQL partout, la même version en local, dans les tests et en production** — décision prise avec Moh Amed : sa pile initiale annonçait SQLite en local, il a tranché pour un moteur unique. Le fichier conserve l'analyse du cas « deux moteurs » sous forme de règle : c'est précisément parce que la base locale qui diffère de la production **cache ses défauts jusqu'au déploiement** (types de colonnes acceptés par l'un et refusés par l'autre, JSON stocké de deux façons, requêtes valides sur un moteur et invalides sur l'autre) qu'un moteur unique est la règle, et non un confort.
- **Multi-tenant** — toute requête sur des données d'organisation est filtrée par organisation ; une requête non filtrée est un défaut critique, pas un oubli. C'est la faille qui tue un SaaS B2B.
- **Migrations** — une migration appliquée ne se modifie jamais, une migration destructrice se demande avant, chaque migration a un `down()` testé, la génération automatique se relit à la main, et les migrations tournent en étape unique du déploiement plutôt qu'au démarrage de chaque instance.
- **Conventions NestJS** — DTO en classes et non en interfaces, validation à la frontière, gardes pour l'autorisation, aucune base atteinte depuis un contrôleur, erreurs de base (`23505`, `23503`) traduites en exceptions Nest.
- **Facturation** — montants en centimes entiers, opérations idempotentes, l'état de l'abonnement fait foi chez le fournisseur de paiement.
- **Données personnelles** — rien de personnel dans les journaux, export et suppression à la demande (RGPD), et une sauvegarde dont la restauration n'a jamais été testée n'est pas une sauvegarde.
- **Déploiement** — conteneur Docker derrière Traefik en HTTPS, secrets par variables d'environnement, jamais dans l'image.

Trois endroits seulement portent des choix de pile (bloc Pile et Commandes, Architecture, section base de données) : si ta pile diffère, ce sont les seuls blocs à ajuster.

## Mise en place

1. **Racine du dépôt**, un seul `AGENTS.md`. Versionné comme le reste du code.
2. **Monorepo** (*rapporté*) : un `AGENTS.md` par zone, avec les consignes propres à cette zone. Veiller à ce qu'ils ne se contredisent pas.
3. **Si un outil lit un autre fichier** : `CLAUDE.md` ou `.cursor/rules/` réduits à un renvoi — « les règles du projet sont dans `AGENTS.md`, lis-le ». Jamais deux copies.
4. **Le faire évoluer à chaque surprise.** Quand un agent fait quelque chose que tu n'attendais pas, la bonne réaction n'est pas de corriger le résultat : c'est d'ajouter la ligne qui manquait. Le fichier devient la mémoire des décisions du projet.

## Comment vérifier qu'il fonctionne

Poser à un agent, dans une session neuve, deux questions dont les réponses ne sont que dans le fichier :

- « Comment lance-t-on un seul test ? »
- « Qu'est-ce que tu dois demander avant de faire ? »

S'il répond correctement, le fichier est lu. S'il improvise, il n'est pas lu, ou il est trop long pour que la partie utile ressorte. Deux minutes de vérification qui évitent des semaines de suppositions.

## Ce qui le rend inefficace

- **Des règles vagues** : « écrire du code propre », « suivre les bonnes pratiques ». Une règle qui n'est pas vérifiable n'est pas une règle.
- **Recopier le guide de style** : le linter le fait mieux.
- **Documenter ce que l'agent sait déjà** : `npm install`, `git commit`.
- **Un fichier de 500 lignes** : la partie utile se noie.
- **Des contradictions** entre la racine et un sous-dossier, ou entre `AGENTS.md` et un `CLAUDE.md` recopié. En cas de conflit, l'agent suit une version au hasard.
- **Mettre la valeur d'un secret dans le fichier** au lieu du nom de sa variable.
- **Des instructions qui changent chaque semaine** : ce qui bouge va dans une note ou un ticket, pas dans le contrat.
- **Aucun niveau « Demander avant »** : l'agent est alors soit paralysé, soit en roue libre.

## À retenir

- `AGENTS.md` est lu par **tous** les agents : c'est le bon endroit pour les frontières, une seule fois.
- La section **Commandes** a le meilleur rendement, avec la commande de test unitaire.
- Les frontières à **trois niveaux** ; « Demander avant » est le niveau qui protège vraiment.
- Une seule source de vérité : les fichiers propres aux outils **renvoient** vers lui au lieu de le recopier.
- Le vérifier par deux questions dans une session neuve, et l'enrichir à chaque surprise.

## Sources

- `cursor.com/docs/context/rules` (consultée le 26/09/2026) : `AGENTS.md` cité à côté des règles de projet ; bonnes pratiques (pas de guide de style recopié, pas de commandes évidentes, pas de cas limites rares)
- `docs.github.com` — fichiers d'instructions du coding agent : `AGENTS.md`, et lecture de `CLAUDE.md` / `GEMINI.md` s'ils existent ; `.github/copilot-instructions.md` (consultées le 26/09/2026)
- Analyse de 2 500 `AGENTS.md` publics, `AGENTS.md` imbriqués en monorepo, limite de 1 000 mots : **rapporté** par des guides tiers
