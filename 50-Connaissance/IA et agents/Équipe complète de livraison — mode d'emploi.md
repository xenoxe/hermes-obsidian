---
tags: [connaissance, ia, agents, orchestration, equipe, cours]
verifie_le: 2026-09-26
---

# Équipe complète de livraison — mode d'emploi

Retour : [[IA et agents — Index]] · Principes : [[Le manager d'équipe — orchestrer des agents]]
Configs prêtes à copier : [[Équipe complète — configs Claude Code]] · [[Équipe complète — configs Cursor]] · [[Équipe complète — configs GitHub Copilot]]

**Rédigé le 26/09/2026.** Sept rôles : un PO, un manager, un dev front, un dev back, un testeur, un relecteur de code et un responsable sécurité. L'objectif n'est pas d'avoir sept agents — c'est d'obtenir du **code propre et fonctionnel**, c'est-à-dire du code qui répond à des critères d'acceptation vérifiés, relu par quelqu'un d'autre que son auteur, et dont la surface de sécurité a été examinée.

## Les sept rôles

Le tableau est le cœur de la note. Ce qui compte n'est pas ce que chaque agent **fait**, c'est ce qu'il **possède** et ce qu'il **refuse** de faire. Une équipe où chacun sait dire non est délégable ; une équipe de généralistes polis ne produit que du bruit.

| Rôle | Possède (écrit) | Produit | Refuse |
|---|---|---|---|
| **PO** `po` | `specs/**` | `specs/<fonctionnalité>.md` : histoires, **critères d'acceptation numérotés**, hors périmètre, contrat d'interface, définition de terminé | d'écrire du code · de démarrer sans critères d'acceptation testables |
| **Manager** `manager` | `PLAN.md`, la branche d'intégration | le plan, les ordres de travail, l'intégration finale | d'écrire du code de production · de fusionner sans avoir lancé les tests lui-même |
| **Dev front** `front-dev` | `src/front/**` | le code d'interface, ses tests de composant, des commits sur sa branche | de toucher au serveur, au schéma de base, à la CI · d'ajouter une dépendance sans accord |
| **Dev back** `back-dev` | `src/api/**`, `migrations/**` | le code serveur, les migrations, les tests unitaires | de toucher à l'interface · de modifier le contrat d'API hors spec |
| **Testeur** `tester` | `tests/**` | les tests **écrits depuis les critères d'acceptation** | de modifier le code de production · d'affaiblir un test pour le faire passer |
| **Relecteur** `code-reviewer` | `reviews/**` | le verdict et les constats par sévérité, avec `fichier:ligne` | d'écrire du code · d'approuver sans avoir lu les tests |
| **Sécurité** `security-reviewer` | `reviews/**` | le rapport par sévérité, avec les points bloquants | d'écrire du code · de conclure sans examiner la surface d'authentification et les entrées |

## Les artefacts sont l'interface

C'est le point que tout le monde rate. **Les agents ne partagent pas la conversation** : un dev front ne sait pas ce que le PO a dit. Ce qui circule entre eux, ce ne sont pas des messages, ce sont des **fichiers dans le dépôt**.

- `specs/<fonctionnalité>.md` (PO) — histoires, critères d'acceptation numérotés `CA-1`, `CA-2`…, hors périmètre, et surtout le **contrat d'interface** : types, signatures, routes, forme des entrées et sorties.
- `PLAN.md` (manager) — la décomposition, les dépendances, qui possède quel fichier, l'ordre.
- Les branches et les commits — le travail lui-même, un par agent.
- `reviews/<fonctionnalité>.review.md` et `reviews/SECURITY-REVIEW.md` — les verdicts.

Conséquence : **le contrat d'interface doit être gelé avant que deux agents partent en parallèle.** Front et back peuvent travailler en même temps seulement si la forme de l'API est écrite noir sur blanc. Sinon ils inventent chacun la leur, et l'intégration devient une négociation.

## L'ordre d'exécution

```
1. PO                    → spec + critères d'acceptation + contrat d'interface
2. MANAGER               → plan, gel du contrat, partition des fichiers
3. front-dev ∥ back-dev ∥ tester
                          (le testeur part en même temps : il écrit depuis les CA,
                           il n'a pas besoin du code pour les écrire)
4. code-reviewer ∥ security-reviewer
                          (tous deux en lecture seule, sur le code fini)
5. MANAGER               → vérification, intégration, fusion
```

Deux remarques importantes.

**L'étape 3 n'est parallèle que si le contrat est gelé.** Si front et back dépendent l'un de l'autre, on enchaîne : back d'abord, puis front contre l'API réellement livrée.

**Le testeur part en même temps que les devs — et c'est délibéré.** Des tests écrits après coup, en lisant le code, ne testent pas la spécification : ils figent ce que le code fait, y compris ses bugs. Des tests écrits depuis les critères d'acceptation disent ce que le code **devait** faire. C'est toute la différence entre une suite de tests et un alibi.

## Le rituel de vérification du manager

À exécuter soi-même, à chaque intégration. Un rapport d'agent est une déclaration, pas une preuve.

```bash
git diff --stat origin/main...HEAD          # ce qui a réellement changé, en volume
git log --oneline origin/main..HEAD         # les commits, et combien par agent
git diff --name-only origin/main...HEAD | grep -E '\.env|\.pem|secret|credential'
                                            # la sortie doit être vide
npm test                                    # soi-même, jamais par ouï-dire
grep -c '^### CA-' specs/<fonctionnalité>.md   # nombre de critères écrits
grep -rno 'CA-[0-9]*' tests/ | sort -u         # quels critères sont couverts
grep -rn 'TODO\|FIXME\|HACK' src/ | wc -l      # ce qui a été laissé en plan
```

Puis la relecture des constats : chaque point « critique » ou « élevé » du rapport de sécurité est-il fermé, ou explicitement accepté avec une raison écrite ? Un point critique ouvert **bloque la fusion**.

## Les frontières du projet

À écrire dans le fichier de consignes commun (`AGENTS.md`, ou l'équivalent selon l'outil), pour que les sept agents le lisent. Le niveau du milieu est celui qu'on oublie partout, et c'est celui qui évite les dégâts :

- **Toujours** — lancer les tests avant de soumettre ; respecter les conventions et l'arborescence existantes ; committer sur sa propre branche ; annoncer dans le rapport les fichiers touchés et les tests lancés.
- **Demander avant** — modifier le schéma de base de données ; ajouter une dépendance ; changer le contrat d'API public ; toucher à la configuration d'authentification, aux migrations ou à la CI.
- **Jamais** — lire ou modifier un fichier d'environnement ou un secret ; `push --force` ; supprimer une branche ; fusionner sans relecture humaine ; désactiver un test pour faire passer la suite.

## Adapter la structure à chaque outil

C'est ici que la plupart des montages échouent, pour une raison technique qu'il faut connaître : **dans Claude Code et Cursor, un sous-agent ne peut pas engendrer d'autres sous-agents.** Il ne parle qu'à son parent. Le « manager » ne peut donc pas être un fichier d'agent comme les autres — il doit être **la session principale** :

| Outil | Où est le manager | Les six autres |
|---|---|---|
| **Claude Code** | La session principale, lancée avec `claude --agent manager` (toute la session prend le prompt du manager) | `.claude/agents/*.md`, appelés par le manager |
| **Cursor** | L'agent principal de la conversation — ses instructions vont dans `.cursor/rules/` | `.cursor/agents/*.md`, disponibles comme outils |
| **GitHub Copilot** | Un agent personnalisé `orchestrateur` qui produit le plan et les ordres de travail — **mais il ne peut pas déclencher les autres** : c'est l'humain qui assigne, sauf à passer par les workflows agentiques | `.github/agents/*.agent.md`, assignés un par un |

Le seul outil des trois qui sait faire coordonner des agents **entre eux** est Claude Code avec ses *agent teams* (expérimental) : un agent meneur y coordonne des coéquipiers qui partagent une liste de tâches et peuvent s'écrire. Partout ailleurs, le manager coordonne et les agents ne se parlent pas.

## Le piège du chiffre sept

**Sept rôles ne veut pas dire sept agents en parallèle.** Le plafond utile est de 2 à 4 agents simultanés. Les sept fichiers constituent un **effectif**, pas un lancement groupé. En pratique, on n'a jamais plus de trois agents actifs : les étapes 1 et 2 sont séquentielles, l'étape 3 en compte trois, l'étape 4 en compte deux.

Et tout l'effectif n'est pas nécessaire à chaque fois :

- **Correction ciblée** → manager + dev concerné + relecteur. Trois fichiers suffisent.
- **Fonctionnalité** → les sept, dans l'ordre ci-dessus.
- **Remaniement** → manager + dev + testeur + relecteur. La sécurité seulement si la surface d'authentification, les entrées ou les dépendances bougent.

Garder les sept fichiers sous la main coûte peu — ils ne consomment rien tant qu'ils ne sont pas invoqués. Les invoquer tous coûte cher.

## Les erreurs qui reviennent

1. **Le testeur qui écrit ses tests depuis le code.** Il fige les bugs. Les tests partent des critères d'acceptation, toujours.
2. **Le relecteur qui relit son propre travail.** Jamais le même agent — et idéalement pas le même modèle ni la même session.
3. **Le PO qui écrit du code « juste pour aider ».** Il vient de perdre son utilité : il n'est plus celui qui juge le résultat.
4. **Deux devs sur le même fichier.** Partitionner par fichier, pas par intention. Le fichier d'index partagé, le schéma et les configurations passent par le manager.
5. **Des critères d'acceptation non testables.** « L'interface doit être agréable » n'est pas un critère. « Un utilisateur non connecté qui ouvre `/tableau` est redirigé vers `/connexion` avec un code 302 » en est un. Chaque `CA-` doit être vérifiable par un test ou une commande, sinon le testeur ne peut rien en faire.
6. **Le manager qui rapporte le travail des agents sans vérifier.** Il devient un perroquet coûteux. Il vérifie sur le disque.
7. **Un fichier critique non figé.** Migrations, schéma, contrats d'API : si deux agents en parallèle les modifient, tout le reste est à refaire.
8. **La sécurité en toute fin, quand il est trop tard pour changer l'architecture.** L'agent sécurité examine **les entrées** et la surface d'authentification ; il doit pouvoir intervenir avant la fusion, pas après la mise en production.

## À retenir

- Chaque rôle a un **périmètre de fichiers** et une **enveloppe de refus** : c'est ça qui rend l'équipe sûre, pas le nombre d'agents.
- Les **artefacts** (`specs/`, `PLAN.md`, `reviews/`) sont l'interface entre agents, parce qu'ils ne partagent pas le contexte.
- Le contrat d'interface se **gèle avant** le travail parallèle.
- Les tests s'écrivent depuis les **critères d'acceptation**, en parallèle des devs.
- Le manager **vérifie sur le disque** : `git diff`, tests lancés par lui, critères comptés, aucun secret touché.
- Sept rôles, **trois agents actifs au plus**. Le reste est un effectif, pas une fan-out.

## Sources

- Champs de sous-agents Claude Code et Cursor : documentation officielle (consultée le 26/09/2026), voir [[Claude Code — agents en équipe]] et [[Cursor — agents en équipe]]
- Format des règles Cursor `.cursor/rules/` (`.md` ou `.mdc`, frontmatter `description`, `globs`, `alwaysApply`) : documentation officielle Cursor, `cursor.com/docs/context/rules`
- Agents personnalisés Copilot et frontières à trois niveaux : documentation GitHub et guides tiers (**rapporté** pour le détail des champs)
- Motifs d'orchestration, plafond de parallélisme, division des rôles : guides tiers convergents et retour d'expérience terrain (**rapporté**)
