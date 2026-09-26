# Vault partagé avec Hermes

Ce dossier est un vault Obsidian lu et écrit par Hermes Agent.

- **Chemin côté Hermes (conteneur)** : `/opt/data/obsidian-vault`
- **Chemin réel côté VPS** : `/docker/hermes-agent-wezi/data/obsidian-vault`
- **Variable d'environnement** : `OBSIDIAN_VAULT_PATH=/opt/data/obsidian-vault`

> Note : le nom `vault` est réservé par Hermes (coffre de mots de passe), d'où `obsidian-vault`.

## Organisation

- `00-Inbox` — notes brutes, captures rapides
- `10-Idees` — idées de contenu, angles, hooks
- `20-Scripts` — scripts et brouillons de vidéos/posts
- `30-Sources` — sources, liens, recherches, citations
- `40-Business` — projets, études de marché, business plan
- `50-Connaissance` — connaissance réutilisable et veille technologique, rangée par domaine → [[Connaissance — Index]]
- `80-Hermes` — espace personnel d'Hermes : fonctionnement de l'installation, conventions, journal de bord → [[Hermes — Index]]
- `90-Templates` — modèles de notes réutilisables

## Convention de liens

Obsidian utilise les wikilinks : `[[Titre de la note]]`.

## Notes de référence

- `30-Sources/Hostinger API/` — documentation complète de l'API Hostinger (392 endpoints, spec OpenAPI 1.54.2) : entrée par [[Hostinger API — Index]]
- `40-Business/Étude SaaS 2026/` — étude de marché SaaS (10 créneaux examinés, notation, plan de validation à 14 jours) : entrée par [[Étude SaaS — Index]]
- `50-Connaissance/IA et agents/` — cours sur le travail en équipe d'agents (Cursor, Claude Code, GitHub Copilot) et le rôle de manager : entrée par [[IA et agents — Index]]

## Test de connexion

Écris une note depuis ton Obsidian, dis-moi son titre, et je la lirai depuis ici.

## Pour les agents

Ce dépôt porte un fichier `AGENTS.md` à sa racine : le contrat lu par tout agent qui travaille dans le vault — commandes de synchronisation, conventions de nommage, frontières (dont l'interdiction absolue de publier un secret) et définition de terminé. À lire avant toute écriture.
