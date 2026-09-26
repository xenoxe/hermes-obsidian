---
tags: [connaissance, ia, agents, claude, cours]
verifie_le: 2026-09-26
---

# Claude Code — agents en équipe

Retour : [[IA et agents — Index]] · Voir aussi : [[Le manager d'équipe — orchestrer des agents]]

**Vérifié le 26/09/2026** sur la documentation officielle Anthropic (`docs.claude.com/en/docs/claude-code/sub-agents`). Les endroits marqués « rapporté » viennent de guides tiers.

## L'idée

Un **sous-agent** est une session Claude Code à part, avec sa propre fenêtre de contexte, son modèle, sa liste d'outils autorisés et son prompt système. Quand la tâche correspond à sa description, Claude lui délègue le travail ; le sous-agent travaille seul et renvoie un résultat.

Le point à bien comprendre, et la documentation officielle insiste dessus : **le sous-agent ne reçoit que son propre prompt système**, plus quelques détails d'environnement (le répertoire de travail). Il ne reçoit **pas** le prompt système complet de Claude Code, et **pas** l'historique de la conversation. Tout ce dont il a besoin doit figurer dans le brief. C'est la source de la plupart des déceptions.

Le sous-agent démarre dans le répertoire de travail de la conversation principale. Attention : dans un sous-agent, un `cd` ne persiste pas d'un appel Bash au suivant et ne change pas la session principale.

## Écrire un sous-agent

Fichier markdown, frontmatter YAML, corps = prompt système.

Emplacements, par ordre de priorité décroissante :

1. `managed` — fichiers déployés par l'administration de l'organisation, dans le répertoire de réglages géré : **ils gagnent** contre tout le reste à nom égal
2. `.claude/agents/` — projet, versionné avec le dépôt : c'est là qu'on met les agents de l'équipe
3. `~/.claude/agents/` — tous les projets de l'utilisateur
4. `plugin` — apportés par les extensions installées, visibles dans l'autocomplétion `@mention` sous leur nom préfixé

Champs de frontmatter réellement utiles :

- `name`, `description` — la description sert au routage automatique, comme dans Cursor
- `tools` — restreindre le jeu d'outils. Un agent de recherche ne doit pas pouvoir écrire ; un agent qui écrit ne doit pas pouvoir atteindre un serveur MCP sans rapport
- `model` — `sonnet`, `opus`, `haiku`, ou un identifiant précis ; à défaut, héritage du parent. Claude peut aussi passer un `model` pour un appel donné, ce qui prime sur le fichier
- `permissionMode` — `default`, `plan`, `acceptEdits`, `dontAsk`, `bypassPermissions`… Monter à `bypassPermissions` sur un agent qui touche à une base de données, une API ou un déploiement est la façon la plus rapide de faire des dégâts
- `maxTurns` — plafond d'itérations. Sans lui, un agent bloqué boucle et brûle le budget
- `isolation: worktree` — **le champ le plus important dès que l'agent écrit des fichiers** : il obtient une copie isolée du dépôt, et ses commandes Bash ou PowerShell s'exécutent dans ce worktree
- `memory` — annuaire persistant qui survit d'une conversation à l'autre : l'agent accumule des connaissances (motifs du code, enseignements de débogage, décisions d'architecture)
- `background` — exécution en arrière-plan par défaut
- `hooks` — voir plus bas

Exemples donnés par la documentation : un **relecteur de code** en lecture seule (outils d'édition exclus), un **débogueur** qui garde l'édition puisque corriger exige d'écrire, un **data scientist** avec `model: sonnet` pour une analyse plus poussée.

## Invocation

- **Routage automatique** — Claude délègue d'après votre demande, la `description` des agents et le contexte. « Use proactively » dans la description encourage le déclenchement.
- **Langage naturel** — nommer l'agent dans la demande ; Claude décide de déléguer ou non
- **`@agent-name`** — garantit que l'agent tourne pour cette tâche
- **Session entière** — `--agent` en ligne de commande, ou le réglage `agent` : toute la session prend le prompt système, les restrictions d'outils et le modèle de cet agent
- `claude agents` liste tous les agents disponibles, tous emplacements confondus
- `--agents '[{"name":"reviewer","description":"..."}]'` définit des agents valables pour la session seulement, jamais écrits sur disque — pratique en script

## Parallélisme, forks et exécution en arrière-plan

- **Parallèle** : plusieurs appels d'agent dans **un même message**. Deux appels dans deux messages successifs s'exécutent l'un après l'autre — le second attend le retour du premier pour même être décidé.
- Les guides tiers convergent sur un plafond pratique de **3 sous-agents simultanés** (certains disent 3 à 5) : au-delà, le parent doit absorber trop de résultats à la convergence et le gain de temps disparaît. Pour plus de tâches indépendantes, grouper par paquets de trois.
- **Fork** : un type de sous-agent particulier qui hérite de **tout** l'historique de la conversation, du même prompt système et des mêmes outils que la session principale — donc du **cache de prompt partagé**, ce qui le rend moins cher au démarrage. Contrepartie : il transporte aussi le contexte inutile. Les spawns sans type précis utilisent l'agent généraliste.
- **Tous les spawns tournent en arrière-plan** (changement récent) : on est notifié à la fin plutôt que bloqué.

## Hooks de cycle de vie

Deux endroits, deux usages :

- **Dans le frontmatter du sous-agent** : les hooks ne vivent que pendant l'exécution de cet agent. `PreToolUse` et `PostToolUse` (avant / après un outil). L'événement `Stop` écrit dans le fichier est automatiquement converti en `SubagentStop` à l'exécution.
- **Dans `settings.json`** : `SubagentStart` et `SubagentStop`, déclenchés dans la session principale, avec un sélecteur sur le nom de l'agent — permet de journaliser, notifier ou contrôler au niveau du projet.

## Agent teams — l'expérience

Les *agent teams* sont une fonctionnalité **expérimentale**, activée par `export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`, puis `claude --agent` (rapporté).

La différence avec les sous-agents est le modèle de communication. Un sous-agent est strictement parent-enfant : il ne parle qu'au parent, et ne peut pas engendrer d'autres agents. Dans une équipe, **un agent meneur coordonne des coéquipiers** qui partagent une **liste de tâches** et une **boîte aux lettres**, et peuvent s'écrire directement entre eux par leur nom (rapporté : outil `SendMessage`). Les définitions d'équipe vivent dans `.claude/agent-teams/`. Des hooks `TeammateIdle` (un coéquipier a fini — lui router la tâche suivante) et `TaskCompleted` permettent de moduler le flux.

Le coût : **beaucoup plus élevé**, chaque coéquipier recevant un contexte complet. Règle prudente : commencer par les sous-agents, et ne passer à l'équipe que quand les agents ont réellement besoin de se parler entre eux. Pour des tâches parallèles simples, les sous-agents suffisent et coûtent moins.

## Ce qu'il reste à savoir

- **Compaction automatique** : les sous-agents ont la leur, avec la même logique que la conversation principale, et `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` s'y applique aussi.
- **Retirer un agent intégré** : `claude --disallowedTools "Agent(Explore)"` — même mécanique pour les autres.
- **Thinking étendu** : hérité de la session depuis v2.1.198, sans réglage par sous-agent.
- **`--append-subagent-system-prompt`** (v2.1.205+) : ajoute un texte à la fin du prompt système de **tous** les sous-agents, y compris imbriqués. Utile en mode non interactif.

## Les erreurs qui coûtent le plus

1. **Déléguer ce qu'on sait faire en une ligne.** Si le chemin du fichier est connu, on lit le fichier. Chaque sous-agent paie un coût de démarrage (lire le `CLAUDE.md`, se réorienter dans le dépôt) avant d'être utile.
2. **Se fier au rapport plutôt qu'au dépôt.** Le parent doit vérifier sur le système de fichiers : `git log`, `git diff`, existence des fichiers, résultat des tests.
3. **Brief incomplet.** Le sous-agent ne peut pas poser de question en cours de route. Un brief ambigu produit un travail plausible et inutile.
4. **Laisser le parent travailler en double.** Chercher les fichiers soi-même puis déléguer la même recherche : on paie deux fois.
5. **Aucune vérification après un fan-out.** Fusionner cinq agents sans lancer l'application ni les tests multiplie les angles morts par cinq.
6. **L'agent d'arrière-plan qui reste bloqué.** En arrière-plan, une autorisation qui exigerait un humain est refusée automatiquement, faute de personne pour l'accorder. Un agent d'arrière-plan doit donc avoir un mode de permission lui permettant de finir seul.

## À retenir

- Sous-agents = markdown dans `.claude/agents/` ; le projet gagne contre l'utilisateur, l'administration gagne contre tout le monde.
- Le sous-agent **ne sait que ce qu'on lui écrit** : il ne voit ni la conversation ni le prompt système complet.
- `isolation: worktree` dès qu'un agent écrit. `maxTurns` sur chacun. Ne pas monter `permissionMode` plus haut que nécessaire.
- Parallèle = plusieurs appels dans **un même message**, plafond pratique de trois.
- Les *agent teams* existent, sont expérimentales, et coûtent nettement plus cher : sous-agents d'abord.

## Sources

- `docs.claude.com/en/docs/claude-code/sub-agents` (documentation officielle, consultée le 26/09/2026)
- Guides tiers convergents sur l'orchestration (plafond de parallélisme, modèle *agent teams* avec `SendMessage`) — **rapporté**, pas confirmé sur la documentation officielle
