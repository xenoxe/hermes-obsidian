---
tags: [infrastructure, audit, hostinger, docker, traefik, signoz]
verifie_le: 2026-09-29
---

# Infrastructure — Audit du 2026-09-29

État de `srv1398132.hstgr.cloud` **relevé le 29 septembre 2026 vers 14 h 50 UTC**, avant toute modification. Retour : [[Plateforme agence web — Index]]

## Méthode, et ce qu'elle ne permet pas

| Moyen | Ce qu'il donne | Portée |
|---|---|---|
| API Hostinger (`/api/vps/v1`) | VM, projets Docker, conteneurs, ports publiés, actions, sauvegardes, firewall, domaines | **fait** |
| Sondes réseau depuis l'extérieur | ce qui répond réellement sur l'IP publique | **fait** |
| Shell sur l'hôte | réseaux Docker internes, volumes, `df`, `ufw`, `fail2ban`, état ACME, version du moteur | **impossible depuis le conteneur Hermes** |

> [!warning] Ce que cet audit ne dit pas
> Le conteneur Hermes n'a **pas de socket Docker** et l'API Hostinger n'expose **ni les réseaux, ni les volumes, ni l'espace disque, ni l'état du pare-feu hôte**. Les sections correspondantes ci-dessous sont donc **déclarées non vérifiées**, pas supposées vides. Le script `scripts/audit-hote.sh` (à lancer sur l'hôte, lecture seule) les complète.

## A. Ce qui est déjà en place

### La machine

| Élément | Valeur (vérifiée) |
|---|---|
| VM | id **1398132** · `srv1398132.hstgr.cloud` · **148.230.114.215** · IPv6 `2a02:4780:7:4aa7::1` |
| Offre | **KVM 2** — **2 vCPU / 8 192 Mo / 102 400 Mo (100 Go)** · centre de données id 15 |
| Gabarit | id 1210 — *Ubuntu 24.04 with Docker and Traefik* · VM créée le 2026-02-18 |
| État | `running` · `actions_lock: unlocked` |
| Pare-feu Hostinger | **aucun** (`firewall_group_id: null`, liste `/firewall` vide) |
| Snapshot | **aucun** (id 0) |
| Sauvegardes automatiques | **2 points** — 2026-09-19 (18,1 Mo) et 2026-09-26 (6,6 Mo), nœud `node967-nl-srv-1-pbs` |

Hors périmètre : `srv788682.hstgr.cloud` (VM 788682, KVM 2, 147.93.53.234) — **jamais touchée**.

### Les 10 projets Docker déployés

| Projet | État | Conteneurs | Ports publiés sur l'hôte |
|---|---|---|---|
| `traefik` | running | 1 | — (`network_mode: host`) |
| `hermes-agent-wezi` | running | 1 | — |
| `signoz-f599` | running | 6 (4 actifs, 2 init terminés) | 32857, 32858, 32859 |
| `omniroute-lnlo` | running | 2 | 32770 |
| `zitadel-vxri` | running | 3 | 32862, 32863 |
| `n8n` | **arrêté** | 1 | — |
| `maxun-36c6` | **arrêté** | 4 | — |
| `paperclip-k5qu` | **arrêté** | 1 | — |
| `docker-registry` | **arrêté** | 1 | — |
| `agent-zero-qhh9` | **arrêté** | 1 | — |

> [!note] Zitadel a changé d'identifiant pendant l'audit
> Le projet s'appelait `zitadel-hz1z` au premier relevé et était devenu **`zitadel-vxri`** au suivant, avec de nouveaux ports d'hôte (32860/32861 → 32862/32863). Un `docker_compose_down` puis `docker_compose_up` a eu lieu à **12 h 03 UTC** sur la VM. Voir [[Plateforme agence web — Index]] : l'identifiant de projet n'est pas stable, la convention de nommage doit en tenir compte.

### Traefik : ce qui tourne réellement

Configuration extraite du projet `traefik` (verbatim) :

```yaml
services:
  traefik:
    image: traefik:latest
    restart: unless-stopped
    network_mode: host
    command:
      - --api.dashboard=false
      - --api.insecure=false
      - --log.level=WARN
      - --providers.docker=true
      - --providers.docker.exposedbydefault=false
      - --entrypoints.web.address=:80
      - --entrypoints.websecure.address=:443
      - --certificatesresolvers.letsencrypt.acme.httpchallenge=true
      - --certificatesresolvers.letsencrypt.acme.httpchallenge.entrypoint=web
      - --certificatesresolvers.letsencrypt.acme.email=${ACME_EMAIL}
      - --certificatesresolvers.letsencrypt.acme.storage=/letsencrypt/acme.json
      - --entrypoints.web.http.redirections.entrypoint.to=websecure
      - --entrypoints.web.http.redirections.entrypoint.scheme=https
    volumes:
      - traefik-letsencrypt:/letsencrypt
      - /var/run/docker.sock:/var/run/docker.sock:ro
```

Le projet porte la variable `ACME_EMAIL` (adresse en `@srv1398132.hstgr.cloud`).

**Bonne nouvelle vérifiée : HTTPS et Let's Encrypt fonctionnent déjà.** Sans certificat préexistant, `signoz-f599.srv1398132.hstgr.cloud` répond **200** avec un certificat **Let's Encrypt valide** (`CN=signoz-f599.srv1398132.hstgr.cloud`, émis le 29/09/2026 09 h 04 UTC, valable jusqu'au 28/12/2026), et la même URL en `http://` renvoie **301** vers HTTPS. La convention `<nom-du-projet>.<TRAEFIK_HOST>` fonctionne donc de bout en bout — c'est un point d'appui, pas un chantier.

**Ce qui manque côté Traefik** (à corriger, aucune n'est bloquante) :

- pas de `--accesslog` → **aucun journal d'accès**, donc rien à ingérer dans SigNoz ;
- pas de `--metrics.prometheus` → **les compteurs HTTP (2xx/4xx/5xx, latence) ne sont pas exportés** ; c'est pourtant le cœur de l'exigence d'observabilité ;
- pas de `--tracing` → aucune trace distribuée issue de Traefik ;
- image **`traefik:latest`** — aucune version épinglée, un redémarrage peut changer de majeure ;
- adresse ACME sur un domaine technique : les notifications d'expiration ne vont nulle part ;
- aucun intergiciel de sécurité (en-têtes, limitation de débit) ;
- tableau de bord désactivé (`--api.dashboard=false`) — c'est prudent, mais l'API de diagnostic est donc inaccessible.

### SigNoz : déjà installé et fonctionnel

Projet `signoz-f599` — 6 conteneurs :

| Conteneur | Image | État |
|---|---|---|
| `signoz-f599-signoz-1` | `signoz/signoz:latest` | running (UI 8080) |
| `signoz-f599-otel-collector-1` | `signoz/signoz-otel-collector:latest` | running (4317, 4318) |
| `signoz-f599-clickhouse-1` | `clickhouse/clickhouse-server:25.12.5` | running, sain |
| `signoz-f599-zookeeper-1` | `signoz/zookeeper:3.7.1` | running, sain |
| `signoz-f599-signoz-init-1` | `alpine:latest` | terminé (normal) |
| `signoz-f599-signoz-migrator-1` | `signoz/signoz-otel-collector:latest` | terminé (normal) |

Points relevés dans la configuration :

- les trois ports de service sont **publiés sur l'hôte** (`ports: - "8080"`, `"4317"`, `"4318"` sans IP) — Docker leur a attribué 32859, 32857, 32858 ;
- ClickHouse tourne avec `SIGNOZ_OTEL_COLLECTOR_CLICKHOUSE_REPLICATION=true` et un **ZooKeeper** dédié : c'est la configuration grappe, inutile (et coûteuse en RAM) sur un nœud unique ;
- **aucune limite de ressources** et **aucune politique de rétention** déclarée → ClickHouse peut remplir les 100 Go ;
- l'UI passe bien par Traefik (`signoz-f599.srv1398132.hstgr.cloud`, HTTPS) **et** reste joignable en direct sur le port 32859.

### Exposition réelle mesurée depuis Internet

Trois tentatives par port, socket TCP, 6 s de délai :

| Port | Service | Résultat |
|---|---|---|
| 22 | SSH | **ouvert** (3/3) |
| 80 | Traefik (redirection) | **ouvert** (3/3) |
| 443 | Traefik (HTTPS) | **ouvert** (3/3) |
| **32859** | **UI SigNoz** | **ouvert (3/3) — HTTP 200 en clair, sans authentification devant** |
| **32770** | **omniroute** | **ouvert (3/3) — HTTP 307** |
| 32857 / 32858 | OTLP gRPC / HTTP | fermé (3/3) |
| 32862 / 32863 | Zitadel API / login | fermé (3/3) |
| 5000, 5678 | registre, n8n | fermé |

> [!warning] Les deux constats de sécurité réels de cet audit
> 1. **L'interface d'administration de SigNoz est accessible publiquement en HTTP non chiffré sur `148.230.114.215:32859`, sans authentification.** Elle contient les journaux, traces et métriques de toute la plateforme — donc potentiellement des données d'exploitation. À traiter en priorité.
> 2. **Il n'y a aucun pare-feu.** Ni groupe Hostinger (liste vide), ni preuve d'un `ufw` hôte. Les ports qui répondent aujourd'hui ne sont filtrés par rien : ils ne le sont que parce que les conteneurs concernés sont arrêtés.
>
> À noter : la liste des ports publiés renvoyée par l'API **ne correspond pas** à ce qui répond réellement (32857/32858 annoncés publiés sur `0.0.0.0`, fermés en vérité). Le métadonnée n'est pas une carte de sécurité fiable — seule une mesure depuis l'extérieur, ou `ss -lntp` sur l'hôte, fait foi.

### Un secret en clair

Le fichier `docker-compose.yml` du projet **`n8n`** contient un **mot de passe d'authentification en clair**, écrit directement dans le YAML et non passé par une variable. Le service est arrêté, ce qui limite l'exposition immédiate, mais la valeur reste lisible par toute personne ayant accès au fichier.

**À faire : régénérer ce mot de passe et le déplacer dans l'environnement du projet.** La valeur n'est volontairement pas recopiée ici — ni dans cette note, ni dans aucune autre.

Pour comparaison, les autres projets font correctement : `signoz-f599`, `omniroute-lnlo` et `hermes-agent-wezi` déclarent leurs secrets comme variables (`${...}`) et les reçoivent par le champ `environment` du projet, hors du fichier compose. C'est le modèle à généraliser.

### Ressources consommées — 7 derniers jours

| Métrique | Min | Moyenne | Max |
|---|---|---|---|
| CPU (% de 2 vCPU) | 1,91 % | **11,16 %** | **100 %** |
| RAM | 1 336 Mo | **2 271 Mo** | **7 764 Mo sur 8 192** |
| Uptime (s) | **923** | 84 993 | 188 791 |

Lecture : la machine est confortable en régime normal (11 % de CPU, 2,2 Go sur 8), mais elle a **déjà frôlé la saturation mémoire** (7,7 Go sur 8,2) et a **redémarré récemment** (uptime minimal de 923 s). SigNoz + ClickHouse + ZooKeeper en est le premier responsable. Toute la question de dimensionnement du plan cible découle de ces trois chiffres. L'espace disque consommé n'est **pas** exposé par l'API — inconnu à ce stade.

### Domaines

Trois entrées au portefeuille, **toutes en `pending_setup` avec `domain: null`** : ce sont les domaines gratuits inclus à l'offre, **jamais activés**. **Aucun domaine réel n'existe sur ce compte.**

Conséquence directe : la convention demandée (`traefik.example.com`, `dokploy.example.com`, …) ne peut pas être appliquée aujourd'hui. L'appui disponible immédiatement est le domaine technique Hostinger, qui fonctionne déjà (voir plus haut).

### Activité récente — le serveur bouge

15 actions récentes sur la VM, dont **11 le jour même** :

| Heure (UTC) | Action |
|---|---|
| 12:03:57 | `docker_compose_up` |
| 12:03:00 | `docker_compose_down` |
| 10:20:01 | `docker_compose_up` |
| 09:58:41 | `docker_compose_up` |
| 09:56:20 → 09:46:31 | 4 × `docker_compose_stop` |
| 09:43:49 / 09:39:59 | `docker_compose_up` × 2 |
| 08:23:17 / 08:17:48 | `docker_compose_up` × 2 |
| 2026-09-28 00:04:27 | `ct_set_limits` |

**À confirmer : qui a fait ces opérations ?** Un audit n'a de valeur que si personne d'autre ne modifie la cible en même temps. Le redémarrage de Traefik à 12 h 03 (le certificat par défaut a été régénéré à cette seconde) et la recréation de Zitadel au même moment en découlent.

## B. Ce qui n'a pas pu être vérifié

À compléter par `scripts/audit-hote.sh`, lancé **sur l'hôte** (lecture seule) :

- version du moteur Docker et de Compose ;
- liste et contenu des **réseaux Docker** ;
- liste, taille et emplacement des **volumes Docker** ;
- **espace disque** réellement consommé (`df -h`, `du`) ;
- état du **pare-feu hôte** (`ufw`, `iptables`), de **fail2ban**, de la configuration **SSH** ;
- contenu de l'état **ACME** de Traefik (`/letsencrypt/acme.json`) — la liste réelle des certificats émis ;
- journaux récents de Traefik.

## Sources

- API Hostinger `https://developers.hostinger.com/api/vps/v1/*` — interrogée le 2026-09-29 (VM, Docker, firewall, domaines, sauvegardes, métriques, actions). Référence : [[Hostinger API — Index]].
- Sondes réseau externes (sockets Python, 3 tentatives, 6 s) depuis le conteneur Hermes — 2026-09-29.
- `openssl s_client` / `curl` sans `-k` pour le certificat `signoz-f599.srv1398132.hstgr.cloud` — 2026-09-29.
- Contenus des fichiers `docker-compose.yml` lus via `GET /docker/{projet}` — 2026-09-29.
