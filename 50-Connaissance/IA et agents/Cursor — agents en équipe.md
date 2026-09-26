---
tags: [connaissance, ia, agents, cursor, cours]
verifie_le: 2026-09-26
---

# Cursor — agents en équipe

Retour : [[IA et agents — Index]] · Voir aussi : [[Le manager d'équipe — orchestrer des agents]]

**Vérifié le 26/09/2026** sur la documentation officielle Cursor (`cursor.com/docs/agent/subagents`, `cursor.com/docs/agent/worktrees`). Les endroits marqués « rapporté » viennent de guides tiers et ne sont pas confirmés par l'éditeur.

## L'idée

Un **sous-agent** est un assistant spécialisé à qui l'agent principal délègue une tâche. Il a **sa propre fenêtre de contexte**, travaille de façon autonome, et renvoie un message final. Le parent n'a pas besoin de voir les 40 lectures de fichiers et les recherches ratées : il ne reçoit que le résultat.

Trois bénéfices, dans l'ordre d'importance réelle :

1. **Isolation du contexte** — une exploration longue n'encombre pas la conversation principale.
2. **Parallélisme** — plusieurs sous-agents travaillent en même temps.
3. **Spécialisation** — un sous-agent « revue de sécurité » peut porter des règles que l'agent généraliste n'applique jamais.

Disponible dans l'éditeur, en CLI et avec les Cloud Agents.

## Écrire un sous-agent

Un fichier markdown, frontmatter YAML, corps = prompt système.

Emplacements (le `.cursor/` l'emporte en cas de conflit de nom) :

- `.cursor/agents/` — projet
- `.claude/agents/` et `.codex/agents/` — projet, compatibilité Claude et Codex
- `~/.cursor/agents/` — tous les projets de l'utilisateur

Champs utiles :

- `name`, `description` (obligatoires)
- `model` : `inherit` (défaut), `fast` (vitesse et coût prioritaires), ou un identifiant précis (`claude-4-sonnet`, `gpt-5-mini`, `claude-opus-4-6`…)
- `readonly: true` — aucune édition de fichier, aucune commande qui modifie l'état : c'est le réglage d'un agent de revue
- `is_background: true` — le sous-agent tourne en arrière-plan sans bloquer le parent

Exemple de l'agent le plus rentable à écrire en premier, le **vérificateur** (`.cursor/agents/verifier.md`) : valider le travail annoncé comme terminé, lancer les tests, et dire ce qui passe contre ce qui manque. Un agent qui vérifie vaut mieux que trois agents qui produisent.

Le plus simple : demander à l'agent de créer le fichier. « Crée un sous-agent dans `.cursor/agents/security-reviewer.md` qui vérifie les vulnérabilités courantes… »

## Le point qui fait marcher ou échouer l'affaire

**La `description` est la logique de routage.** L'agent décide seul de déléguer en lisant le nom et la description de chaque sous-agent. Une description vague (« aide au codage ») signifie un agent jamais utilisé. Ajouter « use proactively » ou « always use for » pour encourager le déclenchement.

Corollaire, et c'est l'anti-pattern numéro un de la documentation : **ne pas créer des dizaines de sous-agents génériques.** Cinquante sous-agents flous rendent la délégation aléatoire et la maintenance pénible. Mieux vaut trois agents précis qu'un catalogue.

## Parallélisme

Pour lancer plusieurs sous-agents en même temps : l'agent émet **plusieurs appels à l'outil Task dans un seul message**. Ils tournent alors simultanément. La formulation naturelle marche : « Passe en revue les changements d'API et mets à jour la documentation **en parallèle** ».

`/multitask` (rapporté) fait tourner les sous-agents de façon asynchrone au lieu de les mettre en file. Les sous-agents d'arrière-plan écrivent leur état en cours d'exécution, ce qui permet de **reprendre** un sous-agent après sa fin pour continuer la conversation avec le contexte préservé.

## Isolation : worktrees et environnements

Chaque sous-agent travaille dans son **propre environnement, avec sa propre branche** — soit un worktree git local (répertoire de travail séparé, même historique), soit un environnement cloud dédié. Les modifications restent sur la branche du sous-agent jusqu'à ce que le parent fusionne. C'est ce qui permet à deux agents d'écrire sans se marcher dessus.

Commandes et réglages associés (rapportés, doc Cursor « Worktrees ») :

- `/worktree`, `/best-of-n` (n tentatives en parallèle, on garde la meilleure), `/apply-worktree`, `/delete-worktree`
- `.cursor/worktrees.json` pour les étapes d'installation propres au projet
- Nettoyage automatique paramétrable : `cursor.worktreeCleanupIntervalHours`, avec un plafond `cursor.worktreeMaxCount` (25 par machine par défaut)
- `git worktree list` pour voir les worktrees existants, `git worktree prune` pour les orphelins

## Agents Window et Cloud Agents

L'Agents Window sert à piloter **plusieurs agents sur plusieurs dépôts et environnements** en parallèle, localement ou dans le cloud, et à suivre un plan décomposé en étapes indépendantes (« Build in Parallel » : les étapes indépendantes partent ensemble, les dépendantes attendent leur tour). Une tâche peut être déplacée du local vers le cloud et inversement, et lancée depuis le web, le mobile, Slack, GitHub ou Linear.

## Ce que ça coûte

La documentation est claire et honnête : les sous-agents consomment des jetons indépendamment. **Cinq sous-agents en parallèle ≈ cinq fois les jetons** d'un seul agent, plus les frais de démarrage (chacun reconstitue son contexte). Sur une tâche courte et simple, l'agent principal est plus rapide. Le tableau officiel :

| Avantage | Contrepartie |
|---|---|
| Isolation du contexte | Coût de démarrage : chaque sous-agent reconstitue son contexte |
| Exécution parallèle | Consommation de jetons multipliée |
| Spécialisation | Latence : plus lent que l'agent principal sur une tâche simple |

## À retenir

- Sous-agents = markdown dans `.cursor/agents/`, compatibles avec les dossiers Claude et Codex.
- La description **est** le mécanisme de routage ; trois agents précis battent cinquante agents vagues.
- `readonly: true` pour tout ce qui observe, `is_background: true` pour ce qui n'a pas besoin de bloquer.
- Parallèle = plusieurs appels Task **dans un même message**.
- Isolation par worktree ou environnement cloud : indispensable dès que deux agents écrivent.
- Le coût en jetons est proportionnel au nombre d'agents. Le gain est en temps, pas en argent.

## Sources

- `cursor.com/docs/agent/subagents` (documentation officielle, consultée le 26/09/2026)
- `cursor.com/docs/agent/worktrees`, pages Agents Window et Cloud Agents (idem)
