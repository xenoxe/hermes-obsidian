---
tags: [connaissance, ia, agents, copilot, github, cours]
verifie_le: 2026-09-26
---

# GitHub Copilot — agents en équipe

Retour : [[IA et agents — Index]] · Voir aussi : [[Le manager d'équipe — orchestrer des agents]]

**Vérifié le 26/09/2026** sur la documentation officielle GitHub (`docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-a-pr`). Les endroits marqués « rapporté » viennent de guides tiers — en particulier **les tarifs, qui changent souvent et doivent être revérifiés** sur la page officielle.

## La distinction qui compte, et que le vocabulaire embrouille

Copilot désigne deux choses différentes par « agent ».

| | **Agent mode** | **Coding agent** (ou cloud agent) |
|---|---|---|
| Rythme | Synchrone, en direct | Asynchrone, en arrière-plan |
| Où | Dans l'IDE (VS Code, JetBrains, Eclipse, Visual Studio 2026) | Dans un environnement GitHub Actions éphémère |
| Comment on démarre | On le pilote dans la conversation | On lui **assigne une issue** ou on lance une tâche depuis le panneau Agents |
| Ce qu'on fait pendant | On regarde et on corrige le cap | Autre chose |
| Ce qu'on récupère | Une session de travail | Une **pull request** avec le code, les tests et une auto-revue |
| Coût | Requêtes premium | Requêtes premium **et** minutes GitHub Actions |

Règle pratique : **agent mode quand on construit** et qu'on veut garder la main ; **coding agent** quand la tâche est bien délimitée (corriger un bug, ajouter des tests, un petit remaniement) et qu'on ne veut pas y consacrer son attention.

L'agent mode est accessible dès l'offre gratuite (plafonnée) ; le coding agent demande une offre payante.

## Démarrer une session

Assigner une issue : dans le menu de droite de l'issue, **Assignees → Copilot**. Une boîte de dialogue propose alors :

- un champ **Optional prompt** pour les consignes particulières (motifs de code à suivre, cadre à utiliser, exigences de test, fichiers à ne pas toucher)
- le **dépôt** où travailler et la **branche** de départ
- le menu **agent** : un agent personnalisé du dépôt, de l'organisation ou de l'entreprise
- le **modèle** utilisé, sélectionnable tâche par tâche

Points d'entrée, tous équivalents : les issues, l'onglet ou le panneau **Agents** (`github.com/copilot/agents`), Copilot Chat dans VS Code, JetBrains, Eclipse ou Visual Studio 2026, le chat Copilot sur GitHub.com, la **CLI GitHub**, GitHub Mobile, le lanceur **Raycast**, un **run d'Actions en échec**, et les intégrations **Jira, Slack, Microsoft Teams, Azure Boards, Linear**. N'importe quel outil de code agentique compatible MCP peut aussi le déclencher — ce qui inclut, en pratique, les autres agents.

## Déclencher depuis un script (API)

Copilot met à disposition une **API d'assignation** (en préversion publique au moment de la vérification). GraphQL d'abord : on vérifie que l'agent cloud est actif sur le dépôt en interrogeant `suggestedActors` — si la fonctionnalité est là, le premier nœud a pour identifiant `copilot-swe-agent`. Puis la mutation `replaceActorsForAssignable`, avec un objet `agentAssignment` :

- `targetRepositoryId` — le dépôt
- `baseRef` — la branche de départ
- `customInstructions` — les consignes
- `customAgent` — l'agent personnalisé à utiliser
- `model` — le modèle

Particularité à ne pas oublier : l'en-tête `GraphQL-Features: issues_copilot_assignment_api_support,coding_agent_model_selection` est **obligatoire**, sinon la requête ne passe pas. Côté REST, les paramètres équivalents sont `target_repo`, `base_branch`, `custom_instructions`, `custom_agent` et `model`. Les mutations GraphQL utilisables : `updateIssue`, `createIssue`, `addAssigneesToAssignable`, `replaceActorsForAssignable`.

## Agents personnalisés

Un agent personnalisé est un fichier markdown avec frontmatter, dans `.github/agents/` (sur la branche par défaut) — nommé `<nom>.agent.md`. Il porte un nom, une description, une liste d'outils autorisés, et éventuellement un bloc `mcp-servers` pour lui brancher un serveur MCP (les secrets correspondants sont passés en variables d'environnement préfixées `COPILOT_MCP_`). Il est assignable depuis le menu déroulant des issues, et définissable au niveau du dépôt, de l'organisation ou de l'entreprise. On peut aussi le créer directement depuis l'interface.

C'est là que se joue la gouvernance : **la liste d'outils restreint ce que l'agent peut faire**. Un agent de revue de sécurité n'a pas accès à l'édition ; un agent de déploiement ne touche pas au code métier.

## Agent HQ et Mission Control — plusieurs fournisseurs ensemble

**Rapporté** (guides tiers, préversion publique depuis février 2026) : GitHub a ouvert un centre de contrôle unique, Mission Control, pour piloter **Copilot, Claude et Codex côte à côte** sur les mêmes dépôts, depuis GitHub, VS Code ou le mobile. Chaque agent reste dans le flux Git normal : il lit les issues, écrit dans une branche, ouvre une PR en brouillon, et répond aux `@mentions` dans les commentaires. Tout passe par la revue habituelle.

Deux points de mise en route qui piègent : les agents tiers sont **désactivés par défaut**, à activer dans Réglages → Copilot → Agent access en acceptant les conditions de chaque fournisseur ; et chaque session d'agent consomme **une requête premium** plus des minutes GitHub Actions.

## Trois façons d'organiser plusieurs agents

1. **Tâches indépendantes en parallèle** — c'est le gain facile. Trois issues distinctes assignées à trois agents, trois branches, aucun conflit.
2. **Comparaison concurrentielle** — la même issue assignée à plusieurs agents, trois PR, on lit, on fusionne la meilleure, on ferme les autres. Particulièrement utile sur les décisions d'architecture. Sous-produit précieux : **si trois agents compétents interprètent l'issue de trois façons différentes, c'est l'issue qui était ambiguë** — c'est une information sur la qualité de la rédaction, pas seulement sur les agents.
3. **Chaîne de relais** — un agent analyse et publie un plan en commentaire, le deuxième implémente, un troisième écrit les tests. On pilote entre les étapes en commentant.

À garder séquentiel : ce qui a des dépendances, l'exploration d'un code inconnu, les problèmes exigeant de valider une hypothèse. À paralléliser : la recherche, la documentation, la revue de sécurité, les modules distincts. **Toujours partitionner par module ou par fichier avant d'assigner**, sans quoi deux agents produisent un conflit de fusion.

## Le fichier qui change tout : AGENTS.md

Un fichier markdown à la racine du dépôt qui décrit la pile technique, les commandes de construction et de test, le style, l'architecture et surtout **les frontières**. Il est lu par **tous** les agents, quel que soit le fournisseur — c'est la généralisation de `CLAUDE.md` à l'ensemble de l'écosystème. On peut en placer dans les sous-dossiers pour donner des consignes différentes selon la partie du dépôt.

À distinguer de `.github/copilot-instructions.md` : le contexte permanent, lu à chaque tâche, à garder **sous 1 000 mots** — au-delà, la partie utile se noie.

La structure qui revient dans les fichiers efficaces (GitHub a analysé plus de 2 500 `AGENTS.md` publics, **rapporté**) : vue d'ensemble du projet · commandes de construction et de test · style de code · architecture et arborescence · standards de test · **frontières**.

Et les frontières, c'est le point qui compte. Le bon format est en trois niveaux, parce qu'il dit à l'agent quoi faire **quand il hésite** :

- **Toujours** — lancer les tests avant de soumettre une PR, suivre les conventions de nommage existantes
- **Demander avant** — modifier le schéma de base de données, ajouter une dépendance
- **Jamais** — toucher aux fichiers `.env` ou aux secrets, forcer un push, supprimer une branche, fusionner sans revue humaine

Encadrer les agents par leur jeu d'outils *et* par ces trois niveaux écrits noir sur blanc, c'est ce qui rend la délégation réellement sûre. Sans cela, on n'a qu'une autocomplétion plus bruyante.

## Bien rédiger une issue

La qualité de la PR produite dépend presque entièrement de la qualité de l'entrée. Un guide tiers propose le cadre **WRAP** (**rapporté**, mais directement utile) :

- **W — What** : ce qui doit changer, et pourquoi
- **R — References** : les fichiers, fonctions ou PR passées pertinentes
- **A — Acceptance criteria** : à quoi ressemble un succès
- **P — Precautions** : ce que l'agent doit éviter

Comparer « Corrige le bug de connexion » à une issue qui nomme le fichier, la fonction, la cause probable, les tests existants à faire passer, le test à ajouter, et interdit explicitement les cookies et la modification du format de jeton : c'est le même agent, ce n'est pas le même résultat.

Et la règle de cadrage : **ne pas assigner une issue ouverte** du type « améliore les performances du code ». L'agent travaille jusqu'à épuisement des consignes ou du quota. Une tâche bien délimitée du backlog, oui ; un chantier sans fin, non.

## Workflows agentiques

**Rapporté** : GitHub teste une automatisation où l'on décrit en markdown ce qu'on veut dans `.github/workflows/`, compilée par `gh aw compile` en vrais fichiers Actions. Principes : **lecture seule par défaut**, écritures seulement via des *safe outputs* pré-approuvés et assainis, exécution en conteneur isolé avec liste d'outils autorisés, et enchaînement de workflows par `dispatch-workflow` — ce qui permet le motif orchestrateur / travailleurs. C'est une préversion technique à surveiller, pas encore une base de production.

## À retenir

- **Agent mode** = on pilote en direct dans l'IDE. **Coding agent** = on assigne une issue, une PR revient.
- Une issue bien écrite (cadre WRAP) pèse plus que n'importe quel réglage.
- `AGENTS.md` est le contrat commun à tous les agents ; les frontières **Toujours / Demander avant / Jamais** sont la partie décisive.
- Les agents personnalisés (`.github/agents/`) encadrent les capacités par leur liste d'outils et leurs serveurs MCP.
- Trois organisations : indépendant en parallèle, comparaison concurrentielle, chaîne de relais. Partitionner par fichier avant d'assigner.
- Coûts : requêtes premium **et** minutes Actions ; une session consomme davantage qu'une simple requête parce qu'elle planifie, implémente, teste et se relit.

## Sources

- `docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-a-pr` (documentation officielle, consultée le 26/09/2026) — modes de démarrage, champ de consignes, API d'assignation
- Guides tiers sur Agent HQ, `AGENTS.md`, le cadre WRAP et les tarifs — **rapporté**, à revérifier avant toute décision d'achat
