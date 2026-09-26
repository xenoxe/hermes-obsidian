---
tags: [connaissance, index, veille]
---

# Connaissance — Index

Branche de connaissance du vault : elle regroupe **toute la veille technologique**, rangée par domaine plutôt que par date. Une idée, un métier, une techno = un domaine ; dans chaque domaine, un index et des notes thématiques.

## Les domaines

- [[IA et agents — Index]] — agents de code, orchestration multi-agents, LLM, outils d'IA
- *(à créer au fil de la veille : développement et outils, cloud et infrastructure, sécurité, produit et SaaS métier, data, web et référencement…)*

## Comment cette branche est rangée

| Dossier | Contenu |
|---|---|
| `Connaissance — Index.md` | ce fichier : la porte d'entrée, la liste des domaines |
| `<Domaine>/<Domaine> — Index.md` | la table des matières du domaine, tenue à jour à chaque ajout |
| `<Domaine>/<Sujet>.md` | une note = un sujet, avec ses sources et sa date de vérification |

## Conventions d'écriture

1. **Une note = un sujet.** Un cours sur trois outils se découpe en trois notes reliées entre elles, pas en un pavé de 20 000 caractères.
2. **Chaque note porte sa date de vérification en tête** et ses sources en pied, avec l'URL. Les outils d'IA et les plateformes changent tous les mois : une note sans date est une note qu'on ne peut pas croire.
3. **Séparer le vérifié du rapporté.** Ce que j'ai lu sur la documentation officielle est affirmé ; ce qui vient d'un guide tiers est marqué comme tel. C'est la même règle que dans [[Étude SaaS — Index]].
4. **Toujours écrire ce qui est faux ou périmé.** Un cours qui se contente de décrire est inutile ; la valeur est dans les pièges, les limites et les cas où la méthode échoue.
5. **Nommer les fichiers sans caractère interdit** (`:` `?` `*` `"` `<` `>` `|` `/` `\`), sans quoi la synchronisation casse selon l'appareil.

## Ce qui ne va pas ici

Le suivi d'un projet en cours, les idées de contenu et les notes datées d'actualité restent respectivement dans `40-Business`, `10-Idees` et `30-Sources`. Cette branche est pour la connaissance **réutilisable** : ce qu'on relit dans six mois parce que c'est encore vrai.

## Notes de cette branche

### IA et agents
- [[Cursor — agents en équipe]] — sous-agents, worktrees, Agents Window, `/multitask`
- [[Claude Code — agents en équipe]] — `.claude/agents/`, isolation worktree, agent teams
- [[GitHub Copilot — agents en équipe]] — agent mode vs coding agent, `.github/agents/`, Agent HQ
- [[Le manager d'équipe — orchestrer des agents]] — le rôle de coordination, commun aux trois outils
