---
tags: [connaissance, ia, agents]
---

# IA et agents — Index

Retour : [[Connaissance — Index]]

Domaine qui rassemble tout ce qui touche aux agents de code, à l'orchestration multi-agents, aux LLM et à leurs outils.

## Cours — faire travailler des agents en équipe

- [[Cursor — agents en équipe]] — sous-agents, isolation par worktree, Agents Window, commandes `/multitask`, `/worktree`, `/best-of-n`
- [[Claude Code — agents en équipe]] — sous-agents en markdown, `isolation: worktree`, exécution en arrière-plan, *agent teams* expérimentaux
- [[GitHub Copilot — agents en équipe]] — agent mode contre coding agent, agents personnalisés, Agent HQ et Mission Control
- [[Le manager d'équipe — orchestrer des agents]] — le rôle de coordination : décomposer, distribuer, vérifier ; ce que les trois outils ont en commun et où chacun diverge

## Une équipe complète, prête à copier

Sept rôles — PO, manager, dev front, dev back, testeur, relecteur de code, sécurité — pour livrer du code propre et fonctionnel.

- [[Équipe complète de livraison — mode d'emploi]] — le rôle de chacun, ce qu'il possède, ce qu'il **refuse** de faire, l'ordre d'exécution, le rituel de vérification du manager, le piège du chiffre sept
- [[Équipe complète — configs Claude Code]] — sept fichiers `.claude/agents/` avec les prompts détaillés, plus `CLAUDE.md`
- [[Équipe complète — configs Cursor]] — les mêmes sept en `.cursor/agents/` (ou réutilisés depuis `.claude/agents/`, que Cursor lit), plus la règle `.cursor/rules/manager.mdc`
- [[Équipe complète — configs GitHub Copilot]] — sept `.github/agents/`, `AGENTS.md`, et les cinq modèles d'issues qui servent de briefs
- [[AGENTS.md — le contrat commun aux agents]] — le fichier lu par **tous** les agents : les huit sections, les frontières à trois niveaux, le modèle à copier et un exemple rempli

**À lire une fois dans l'ordre** : [[Le manager d'équipe — orchestrer des agents]] (principes, valables partout), puis [[Équipe complète de livraison — mode d'emploi]] (les sept rôles et leurs contrats), puis la note de configuration de l'outil utilisé. Les trois cours de plateforme se lisent indépendamment.

## À compléter au fil de la veille

- Modèles et fournisseurs : ce qui change réellement d'un modèle à l'autre sur les tâches longues
- MCP (Model Context Protocol) : brancher des outils externes sur un agent
- Évaluation : comment mesurer qu'un agent fait bien son travail, et pas seulement qu'il rend une réponse
- Coût et contexte : ce qui fait exploser la facture, et les stratégies de compactage
