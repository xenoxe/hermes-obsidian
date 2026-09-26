> **Modèle à copier dans un dépôt de code.** Ce dossier ne s'applique pas au vault où il est rangé.

# Instance complète — équipe d'agents pour un SaaS NestJS + Vue/React + PostgreSQL

Arborescence prête à copier **telle quelle** dans un dépôt de code, à la racine. Sept rôles, trois outils, les règles, les modèles d'issues.

## Comment copier

Depuis le clone local du vault, sur ton PC. **Attention : Obsidian masque les dossiers qui commencent par un point** (`.claude`, `.cursor`, `.github`) — ils sont bien présents sur le disque et synchronisés par Git, mais tu ne les verras pas dans l'interface d'Obsidian. Ouvre le dossier avec l'**Explorateur Windows**, ou affiche les éléments cachés.

À copier à la racine du dépôt du projet :

```
AGENTS.md                 → la racine du dépôt          (le contrat, la source unique)
CLAUDE.md                 → la racine du dépôt          (renvoi pour les outils qui lisent ce nom)
.claude/                  → la racine du dépôt          (agents pour Claude Code)
.cursor/                  → la racine du dépôt          (agents + règles pour Cursor)
.github/                  → fusionner avec l'existant  (agents Copilot, instructions, modèles d'issues)
specs/  reviews/          → la racine du dépôt          (dossiers de travail de l'équipe)
```

`.github/` peut déjà exister dans ton dépôt (workflows) : copie son **contenu** à l'intérieur plutôt que de remplacer le dossier.

## Ce qu'il y a dedans

| Chemin | Rôle |
|---|---|
| `AGENTS.md` | Le contrat lu par **tous** les agents : pile, commandes, architecture, conventions, base de données, sécurité, frontières, définition de terminé |
| `CLAUDE.md` | Quatre lignes de renvoi vers `AGENTS.md` — jamais de contenu dupliqué |
| `.claude/agents/` | **7 fichiers d'agents** avec leur frontmatter complet (`tools`, `model`, `permissionMode`, `maxTurns`, `isolation: worktree`) |
| `.cursor/agents/` | **6 fichiers** — même corps de prompt, frontmatter propre à Cursor (`readonly`, `is_background`) |
| `.cursor/rules/` | **6 règles** : doctrine du manager, frontières, API NestJS, base de données, interface, tests |
| `.github/agents/` | **7 agents Copilot** (`.agent.md`), assignables à une issue |
| `.github/copilot-instructions.md` | Le contexte permanent, court, qui renvoie vers `AGENTS.md` |
| `.github/ISSUE_TEMPLATE/` | **5 modèles d'issues** suivant le cadre WRAP — c'est le brief, côté Copilot |
| `specs/` | Les spécifications produites par le PO (avec les critères d'acceptation `CA-1`, `CA-2`…) |
| `reviews/` | Les rapports du relecteur et du responsable sécurité |

## Le point à comprendre sur le manager

Dans **Claude Code** comme dans **Cursor, un sous-agent ne peut pas en engendrer d'autres** : il ne parle qu'à son parent. Le manager n'est donc pas un agent comme les autres.

- **Claude Code** — `.claude/agents/manager.md` existe, mais il s'utilise comme **la session** : `claude --agent manager`. Toute la session prend son prompt, ses permissions et son modèle. Il appelle ensuite les six autres.
- **Cursor** — il n'y a pas de `manager.md` dans `.cursor/agents/`, volontairement : le manager est **l'agent principal de la conversation**, et sa doctrine vit dans `.cursor/rules/00-manager.mdc` (appliquée à chaque session). Les six autres agents sont disponibles comme outils.
- **GitHub Copilot** — `orchestrateur.agent.md` produit le plan et les ordres de travail, mais il **ne peut pas déclencher les autres** : c'est toi qui assignes chaque issue. `.github/ISSUE_TEMPLATE/` sert exactement à ça.

## Ce qu'il faut remplir avant d'utiliser

1. `AGENTS.md` — remplacer `<Nom du SaaS>` par le nom du produit (seule case vide du fichier).
2. `AGENTS.md` — vérifier que les commandes correspondent réellement à ton `package.json` : installer, développer, tester, lancer **un seul** test, migrations. C'est la section au meilleur rendement, et une commande fausse y est pire que rien.
3. `AGENTS.md` et `.cursor/rules/30-base-donnees.mdc` — la version de PostgreSQL doit être **la même** en local, dans les tests et en production.
4. `.github/agents/*.agent.md` — **vérifier les noms d'outils** du frontmatter dans l'éditeur d'agents de GitHub, qui écrit le fichier correctement. Les listes fournies suivent des sources convergentes, pas la documentation officielle complète.
5. Supprimer, dans `AGENTS.md`, la section « Si ta pile diffère » une fois la pile fixée.

## Mise en route

```bash
mkdir -p specs reviews          # déjà présents ici, à recréer dans ton dépôt si besoin
cp -r .claude .cursor .github AGENTS.md CLAUDE.md /chemin/vers/ton-projet/
cd /chemin/vers/ton-projet
claude --agent manager          # ou ouvrir Cursor ou VS Code
```

Puis : « Lis `specs/<fonctionnalité>.md` — s'il n'existe pas, fais produire la spécification par le PO — et exécute le plan. »

## Vérifier en deux minutes que les agents lisent bien les règles

Dans une session neuve, poser ces deux questions, dont les réponses ne sont **que** dans les fichiers :

1. « Comment lance-t-on un seul test côté API ? »
2. « Qu'est-ce que tu dois demander avant de faire ? »

Réponses correctes → les règles sont lues. Réponses inventées → elles ne sont pas lues, ou le fichier est trop long pour que la partie utile ressorte.

## Ne pas laisser diverger les copies

`AGENTS.md` est la **source unique**. Les fichiers propres à chaque outil n'existent que pour les réglages que lui seul comprend (`readonly` et `is_background` chez Cursor, `tools` et `isolation` chez Claude Code, `tools` chez Copilot). Si une règle change, elle change dans `AGENTS.md` **et** dans la règle de même sujet — jamais dans l'un sans l'autre : deux versions divergentes des mêmes règles valent moins que zéro, parce qu'un agent suivra alors celle qu'il a lue en dernier.
