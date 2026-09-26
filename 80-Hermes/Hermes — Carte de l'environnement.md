---
tags: [hermes, environnement, meta]
---

# Hermes — Carte de l'environnement

Où sont les choses au 2026-09-26. Retour : [[Hermes — Index]]

## Vue d'ensemble

Hermes Agent tourne dans un **conteneur Docker** hébergé sur le VPS de Moh Amed, et lui parle via **Telegram**. Le disque du conteneur est éphémère ; seule `/opt/data` survit (c'est le volume partagé avec l'hôte).

| Élément | Valeur |
|---|---|
| Hôte | `srv1398132.hstgr.cloud` — VM id **1398132**, IP 148.230.114.215 (Ubuntu 24.04 + Docker + Traefik) |
| Conteneur | `273f1a21f5d4` (image Hermes, Debian 13), nom côté hôte `hermes-agent-wezi` |
| Volume partagé | conteneur `/opt/data` ↔ hôte `/docker/hermes-agent-wezi/data` |
| Version Hermes | v0.21.5 (2026.9.24) |
| Profils | `default` (celui-ci) + `content-creator` (veille de tendances, headless) |
| Passerelle | une seule passerelle sur l'hôte, multiplexée pour les deux profils |

> [!warning] Deux choses que le conteneur ne peut pas faire
> - **Pas de socket Docker** : je ne peux pas lancer de conteneur depuis l'intérieur. Pour deployer, on passe par l'API Hostinger ([[Hostinger API — Index]]) ou par une commande que Moh Amed exécute sur l'hôte.
> - **Pas de terminal interactif** : les onglets de terminal en arrière-plan sont en lecture seule pour lui ; une saisie interactive (login, prompt de 2FA) doit se faire depuis un terminal hôte avec `docker exec -it`.

## Chemins utiles

| Chemin (conteneur) | Rôle |
|---|---|
| `/opt/data/obsidian-vault` | ce vault |
| `/opt/data/.env` | secrets et variables d'environnement (jamais recopié ailleurs) |
| `/opt/data/config.yaml` | configuration Hermes |
| `/opt/data/bin/vault-sync.sh` | script de synchronisation du vault |
| `/opt/data/bin/secret-patterns.txt` | motifs de détection de secrets (source unique) |
| `/opt/data/scripts/` | scripts exécutables par les tâches planifiées |
| `/opt/data/cache/scratch` | brouillons et fichiers temporaires |

## Tâches planifiées

| Nom | Rythme | Rôle |
|---|---|---|
| `obsidian-vault-sync` (id `5f2882ca2c8b`) | toutes les 15 min | `pull` + `commit` + `push` du vault ; silencieux si tout va bien, ne parle qu'en cas de problème |
| `web-trend-scan` (profil `content-creator`) | toutes les 6 h | écrit des digests de tendances dans `/opt/data/content-ideas/` |

## Accès externes

- **Telegram** : canal principal (ce chat).
- **API Hostinger** : `https://developers.hostinger.com`, clé dans `/opt/data/.env` → `HOSTINGER_API_KEY`. Le détail des endpoints est dans [[Hostinger API — Index]].
- **GitHub** : `xenoxe/hermes-obsidian` (privé), écriture via la clé de déploiement `/opt/data/.ssh/id_ed25519_obsidian`.

## Règle de périmètre

Déploiement **uniquement** sur `srv1398132.hstgr.cloud` (VM 1398132). `srv788682.hstgr.cloud` (VM 788682) est **hors périmètre définitivement** — aucune écriture, même sur demande explicite. Détail dans [[Hostinger API — VPS]].
