---
tags: [connaissance, ia, agents, orchestration, cours]
verifie_le: 2026-09-26
---

# Le manager d'équipe — orchestrer des agents

Retour : [[IA et agents — Index]] · Cours de plateforme : [[Cursor — agents en équipe]] · [[Claude Code — agents en équipe]] · [[GitHub Copilot — agents en équipe]]

**Rédigé le 26/09/2026.** Les principes de cette note sont communs aux trois outils ; ce qui relève d'un outil précis est signalé. Les affirmations issues de guides tiers sont marquées « rapporté ».

## Le rôle, en une phrase

Le manager **décompose, distribue, vérifie et intègre**. Il ne fait pas le travail : il fait en sorte que le travail se fasse, et il refuse de croire sur parole.

C'est la différence entre « j'ai cinq agents » et « j'ai une équipe ». Cinq agents mal coordonnés produisent cinq fois plus de travail à relire, pas cinq fois plus de travail utile.

## La contrainte fondatrice

**Un sous-agent ne sait que ce qu'on lui écrit.**

Il n'a pas accès à la conversation du manager, ni à ce que le manager a lu. Il ne peut pas poser de question en cours de route. Un brief qui laisse une ambiguïté ne produit pas une erreur : il produit un travail **plausible et faux**, ce qui est bien pire, parce qu'il faut le relire pour s'en apercevoir.

Trois conséquences pratiques :

1. **Écrire le contrat avant de distribuer.** Signatures, types, format d'entrée et de sortie, critères d'acceptation. Si deux agents doivent s'articuler, le contrat doit exister avant que le premier commence.
2. **Passer des chemins de fichiers, pas des contenus.** Le brief dit « travaille dans `src/api/routes.ts`, voici la fonction concernée » — pas 400 lignes collées dans le message.
3. **Le brief doit être autosuffisant.** Le test : un collègue qui n'a jamais vu le projet pourrait-il exécuter la consigne sans rien demander ? Si non, le brief est incomplet.

## Les quatre compétences du manager

**1. Découper selon les dépendances.** Trier ce qui peut aller en parallèle et ce qui doit s'enchaîner. Le test est simple : l'agent B a-t-il besoin du résultat de l'agent A ? Si oui, séquentiel. Si les deux écrivent dans le même fichier, séquentiel aussi — ou partitionner autrement. Paralléliser ce qui est couplé garantit un conflit de fusion.

**2. Spécifier.** Écrire le brief, le contrat, les critères d'acceptation, et nommer explicitement les fichiers que l'agent a le droit de toucher. Un agent sans périmètre écrit élargit son périmètre tout seul.

**3. Partitionner.** Un seul écrivain par fichier à un instant donné. Cela vaut surtout pour les fichiers que tout le monde touche : index, fichiers de configuration partagés, listes de liens, schémas. Ces fichiers-là passent par le manager ou par un agent de fusion désigné, jamais par les travailleurs en parallèle.

**4. Vérifier.** Sur le disque, pas dans le rapport. Voir plus bas : c'est le point où la plupart des équipes d'agents se trompent.

## Le budget de contexte du manager

Le manager est la ressource rare. Chaque résultat d'agent qu'il absorbe en entier le rapproche de la saturation. Les guides tiers convergent sur une cible : **le manager doit rester sous 20 % de sa fenêtre de contexte.**

Ce qui s'ensuit :

- Ne pas lire les fichiers source soi-même si un agent est payé pour ça ; ne pas déléguer la recherche **et** la faire.
- **Ne pas recopier les résultats** dans son propre contexte : les résumer, et pointer vers le fichier ou la branche où le détail vit.
- Un sous-agent doit rendre une synthèse courte, pas un journal. Un résultat de 500 lignes qu'on rapatrie annule le bénéfice de l'isolation.
- Le manager synthétise : des résultats dispersés ne sont pas une réponse.

## Les cinq motifs d'orchestration

Ce sont les briques réutilisables ; le vrai travail les combine.

| Motif | Principe | Quand |
|---|---|---|
| **Chaîne** | Une tâche traverse les agents en séquence | Flux de données clair, étapes dépendantes. Lent, et un maillon faible casse tout |
| **Parallèle** | Tâches indépendantes en même temps, résultats combinés | Gain de temps maximal. Coût en jetons plus élevé, pas de partage intermédiaire |
| **Routage** | Un classificateur envoie chaque demande au spécialiste | La description des agents **est** la logique de routage |
| **Orchestrateur-travailleurs** | Le manager décompose, les travailleurs enquêtent, le manager synthétise | Problèmes multi-domaines |
| **Évaluateur-optimiseur** | Un agent produit, un **autre** évalue, la critique revient, on régénère | Quand la qualité compte plus que la vitesse |

Et une séquence complète qui marche bien sur du code : **scoute → implémente → vérifie**. Un agent d'exploration (lecture seule) établit l'état des lieux et produit un plan ; les implémentations partent en parallèle ; puis un agent de vérification, séquentiel, contrôle l'ensemble. L'étape de vérification est séquentielle **exprès** : elle a besoin du travail fini pour avoir un sens.

## Isolation : la règle qui évite 90 % des problèmes

**Tout agent qui écrit des fichiers travaille dans son propre worktree git.** Tout agent en lecture seule n'en a pas besoin.

C'est la règle la plus simple et la plus rentable de tout ce domaine. Sans isolation, deux agents qui écrivent dans le même répertoire se marchent dessus : le second écrase le premier, et l'état final dépend de l'ordre d'arrivée. Avec un worktree par agent, les deux peuvent écrire le même nom de fichier sans se détruire — le manager réconcilie à l'intégration.

Dans les trois outils, cela se règle par un champ de fichier — `isolation: worktree` côté Claude Code, `readonly: true` et environnements isolés côté Cursor, environnements Actions séparés côté Copilot. Côté Claude Code, la documentation officielle précise que **les commandes Bash de l'agent s'exécutent dans son worktree** : l'isolation couvre aussi les effets de bord de la ligne de commande, pas seulement les fichiers.

Deux précautions : nettoyer les worktrees orphelins (`git worktree list`, `git worktree prune`) — les échecs en laissent derrière eux — et ne pas partager un même worktree entre deux agents qui écrivent.

## Une PR par agent

Règle simple, énorme en pratique : **chaque agent ouvre exactement une pull request**.

Pourquoi : un diff d'un seul agent se relit en isolation ; un diff de trois agents fusionnés ne se relit plus. L'intégration continue tourne par PR, ce qui donne un signal par agent. Et un agent cassé se ferme sans défaire le travail des autres. Quand tout doit absolument atterrir ensemble, on passe par une branche d'intégration : chaque PR y entre, puis un seul commit va vers la branche principale.

## Le plafond de parallélisme

Contre-intuitif mais constant dans toutes les sources : **plus d'agents ne veut pas dire plus vite.** Claude Code et les guides tiers situent le bon régime entre **2 et 4 agents simultanés** — une limite pratique de trois revient le plus souvent.

Raison : au-delà, le coût de coordination (résoudre les conflits, réconcilier des résultats qui divergent, absorber les sorties) dépasse le temps gagné. Et chaque agent supplémentaire repaie le coût de démarrage : lire les consignes communes, se réorienter dans le dépôt. Cinq agents, c'est cinq fois les jetons, pas cinq fois la vitesse.

Pour plus de tâches que le plafond : des paquets de trois, en séquence.

## La gouvernance minimale

Trois éléments, tous les trois nécessaires.

**1. Un périmètre d'outils par agent.** Un agent de recherche ne doit pas pouvoir écrire. Un agent de revue ne doit pas pouvoir modifier. Un agent qui touche au déploiement ne doit pas pouvoir atteindre des outils sans rapport. La restriction est dans les fichiers, pas dans la bonne volonté de l'utilisateur.

**2. Des frontières écrites à trois niveaux.** Dire à un agent quoi faire **quand il hésite** :

- **Toujours** : lancer les tests avant de soumettre, respecter les conventions existantes
- **Demander avant** : modifier un schéma, ajouter une dépendance, toucher à l'infrastructure
- **Jamais** : fichiers d'environnement et secrets, `push --force`, supprimer une branche, fusionner sans revue humaine

Le niveau « demander avant » est celui qui manque partout, et c'est celui qui évite les dégâts : il capture le cas où l'agent est compétent mais sort du cadre prévu.

**3. Des refus explicites, par agent.** Le principe qui ressort d'un retour d'expérience terrain (rapporté) : un agent ne dit pas seulement ce qu'il possède, il dit **ce qu'il refuse de faire**. « Cet agent possède le déploiement et refuse de toucher au code métier. » « Cet agent écrit les tests et ne modifie jamais `src/` . » Un système où chaque agent déclare son enveloppe de refus devient délégable ; sans cela on n'a qu'un assistant plus bavard.

Et deux garde-fous de sécurité : **plafonner les tours** (`maxTurns`) — sans limite, un agent bloqué boucle et consomme — et **ne jamais monter le mode de permission au maximum** sur un agent qui touche à une base de données, une API ou un déploiement.

## Les trois modes d'échec

L'analyse de GitHub (rapportée) propose le bon cadre mental : **traiter les agents comme un système distribué, pas comme une conversation.** Tout le reste en découle.

| Mode d'échec | Ce qui se passe | Remède |
|---|---|---|
| **Échange de données bruité** | Un agent produit du JSON, le suivant attend de la prose | Contrats de données typés ; MCP pour formaliser les entrées et sorties |
| **Actions conflictuelles** | Un agent ferme l'issue qu'un autre vient d'ouvrir ; deux agents saccagent le même fichier | Partitionner : un module, un dossier, un ensemble de fichiers par agent, écrit dans les consignes |
| **Dérive du périmètre** | On demandait une correction de bug, on obtient un module remanié, trois tests cassés et une dépendance ajoutée | Frontières à trois niveaux, écrites noir sur blanc |

## La vérification : le point où tout se joue

**Un rapport d'agent est une déclaration, pas une preuve.** Un agent qui écrit « terminé, tout passe » peut s'être trompé, avoir testé un autre fichier, ou avoir oublié de lancer quoi que ce soit. Ce n'est pas de la mauvaise foi : c'est un système qui produit la réponse la plus plausible.

Ce que le manager vérifie lui-même, sur le disque :

- `git log` et `git diff` — qu'est-ce qui a réellement changé ?
- existence des fichiers annoncés comme créés
- exécution des tests, soi-même
- les totaux annoncés : si un agent dit « 12 fichiers traités », on compte les fichiers

Et la règle qui suit : **ne pas fusionner un fan-out sans avoir lancé l'application ou la suite de tests.** Fusionner cinq agents sans vérification multiplie les angles morts par cinq — on ne divise pas le risque en distribuant le travail, on le répartit.

## Le rôle de manager dans les trois outils

| | **Cursor** | **Claude Code** | **GitHub Copilot** |
|---|---|---|---|
| Où vit le manager | L'agent parent ; Agents Window pour plusieurs en parallèle | La session principale ; *agent teams* pour un meneur nommé | Un agent personnalisé « orchestrateur » ; les workflows agentiques pour l'enchaînement |
| Comment on le définit | `.cursor/agents/` | `.claude/agents/` | `.github/agents/*.agent.md` |
| Isolation par défaut | Environnement et branche propres à chaque sous-agent | `isolation: worktree` sur le champ de l'agent | Environnement Actions distinct par agent |
| Communication entre agents | Non — seulement avec le parent | Non en sous-agents ; **oui** en *agent teams* (boîte aux lettres, messages par nom) | Via issues et commentaires de PR ; `@mentions` |
| Le levier de gouvernance | `readonly`, `model`, description de routage | `tools`, `permissionMode`, `maxTurns`, `isolation`, hooks | `tools` + `mcp-servers` de l'agent, `AGENTS.md` |
| Maturité | Éditeur, CLI, cloud | Sous-agents stables ; *teams* expérimentales | Agent mode stable ; agent cloud et API en préversion publique |

Différence de fond : **Claude Code est le seul des trois à offrir une communication directe entre agents** (en expérimental), et **Copilot est le seul à faire vivre les agents dans un système de tickets et de PR existant** plutôt que dans une session. Cursor se distingue par l'intégration de l'isolation et la conduite de plusieurs agents depuis une fenêtre unique.

## Mise en route progressive

Ne pas commencer par cinq agents. L'ordre qui marche :

1. **Un manager et un vérificateur.** `.cursor/agents/verifier.md` côté Cursor, un sous-agent de revue en lecture seule côté Claude Code, un agent de revue côté Copilot. Rien qu'avec ça, la qualité monte déjà.
2. **Deux ou trois travailleurs spécialisés**, avec des descriptions précises et des périmètres de fichiers disjoints.
3. **L'isolation par worktree** dès le premier agent qui écrit.
4. **Un fichier de consignes commun** (`AGENTS.md` ou équivalent) avec les frontières à trois niveaux.
5. **La mesure** : combien de temps réellement gagné, combien de jetons réellement dépensés. Une équipe d'agents qui n'est pas mesurée coûte sans qu'on le sache.

## Quand ne pas faire d'équipe

- La tâche tient en une lecture de fichier ou une modification précise : le manager la fait, ou un seul agent.
- Toutes les tâches touchent le même fichier : elles sont séquentielles, l'équipe n'apporte rien.
- Le besoin est de **comprendre**, pas de produire : une exploration, une synthèse. Un agent suffit souvent.
- Le budget est serré : le gain est en temps, la dépense en jetons. Paralléliser, c'est payer pour aller plus vite, pas pour dépenser moins.

## À retenir

1. Le manager décompose, distribue, **vérifie**, intègre. Il ne code pas, et il ne croit pas les rapports.
2. Le sous-agent ne sait que ce qu'on lui écrit : le brief doit être autosuffisant, et le contrat écrit avant de distribuer.
3. Un seul écrivain par fichier, un worktree par agent qui écrit, une PR par agent.
4. Plafond de 2 à 4 agents en parallèle — au-delà, le coût de coordination mange le gain.
5. Les frontières à trois niveaux (Toujours / Demander avant / Jamais) et l'enveloppe de refus de chaque agent sont ce qui rend la délégation sûre.
6. Vérifier sur le disque, jamais sur la déclaration : `git diff`, tests, comptage.

## Sources

- Documentation officielle Cursor et Anthropic sur les sous-agents (consultée le 26/09/2026) — champs `isolation: worktree`, restriction d'outils, routage par description
- Guides tiers convergents sur l'orchestration multi-agents : plafond de parallélisme, budget de contexte du manager (< 20 %), motifs d'orchestration, une PR par agent, scoute-implémente-vérifie — **rapporté**
- Analyse GitHub des modes d'échec des systèmes multi-agents, et cadre WRAP pour la rédaction des issues — **rapporté**
- Retour d'expérience terrain sur les refus explicites par agent (laboratoires Azure / GitHub Copilot) — **rapporté**
