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
- `90-Templates` — modèles de notes réutilisables

## Convention de liens

Obsidian utilise les wikilinks : `[[Titre de la note]]`.

## Test de connexion

Écris une note depuis ton Obsidian, dis-moi son titre, et je la lirai depuis ici.
