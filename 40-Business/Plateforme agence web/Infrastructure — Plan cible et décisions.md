---
tags: [infrastructure, plan, hostinger, docker, traefik, signoz, agence]
verifie_le: 2026-09-29
statut: en attente de validation
---

# Infrastructure — Plan cible et décisions

Plan proposé le **29 septembre 2026**, à partir de [[Infrastructure — Audit du 2026-09-29]]. **Rien n'a été modifié.** Retour : [[Plateforme agence web — Index]]

## 1. Ce que la cible devient, tenant compte de l'existant

La cible demandée reste bonne sur le fond. Trois de ses briques **existent déjà** et doivent être réutilisées plutôt que réinstallées, et deux entrent en conflit avec l'existant.

```
                        INTERNET
                            |
                  [ pare-feu Hostinger : 80, 443, SSH restreint ]
                            |
                    +-------+--------+
                    |    Traefik     |  <-- DEJA EN PLACE, a durcir
                    |  80 / 443 TLS  |
                    +-------+--------+
                            |
      +---------------------+---------------------+
      |                     |                     |
  client-a              client-b              client-c
  web + api             web + api             web + api
      |                     |                     |
  internal-a            internal-b            internal-c
  (postgres)             (postgres)            (postgres)
      |                     |                     |
      +---------------------+---------------------+
                            |
                    +-------+--------+
                    |     SigNoz     |  <-- DEJA EN PLACE, a plafonner
                    | metrics/logs/  |
                    | traces/APM     |
                    +----------------+
                            ^
        +-------------------+--------------------+
        |            |             |             |
   OTel Collector  Traefik      applications   Collecteur d'hote
   (4317/4318)     (accesslog,   (instrumentees (CPU/RAM/disque)
                    metrics)      OTel)
                            
   Uptime Kuma  -> disponibilite
   Dozzle       -> debogage Docker rapide
   Backrest + Restic -> sauvegardes chiffrees
   Trivy        -> securite des images (CI + hote)
   Umami        -> statistiques web, un site par client
```

| Brique | Statut face à l'existant |
|---|---|
| **Traefik** | **déjà en place** → on le garde, on le durcit |
| **SigNoz** | **déjà en place** → on le garde, on le plafonne et on le sécurise |
| **Docker Engine** | **déjà en place** (gabarit Ubuntu 24.04 with Docker and Traefik) |
| **Zitadel** | **déjà en place** → à réutiliser comme fournisseur d'identité pour protéger les consoles, au lieu de multiplier les mots de passe partagés |
| **Dokploy** | **à NE PAS installer** — voir le conflit en §7 |
| Uptime Kuma, Dozzle, Backrest, Trivy, Umami | à installer |

## 2. Installations nécessaires

| # | Composant | Forme | Justification |
|---|---|---|---|
| 1 | **Pare-feu Hostinger** | groupe + règles 22/80/443 | aucune protection aujourd'hui ; c'est la correction la plus rentable |
| 2 | **Durcissement Traefik** | modification du projet existant | journal d'accès, métriques HTTP, traces, version épinglée, en-têtes de sécurité |
| 3 | **SigNoz — plafonds et rétention** | modification du projet existant | aujourd'hui : aucune limite, aucune rétention, réplication inutile |
| 4 | **Collecteur d'hôte OTel** | projet `otel-host` | CPU/RAM/disque/réseau/conteneurs → SigNoz (exigence §9) |
| 5 | **Uptime Kuma** | projet `uptime-kuma` | disponibilité + pages de statut par client |
| 6 | **Dozzle** | projet `dozzle` | journaux Docker, derrière authentification |
| 7 | **Backrest + Restic** | projet `backrest` | sauvegardes chiffrées, rétention, vérification, restauration |
| 8 | **Umami + PostgreSQL dédié** | projet `umami` | statistiques, un site par client |
| 9 | **Trivy** | **pas de service** : étape de CI GitHub Actions + script hôte | analyse d'images, secret et configuration |
| 10 | **fail2ban** | paquet hôte | SSH exposé sur Internet |

## 3. Ports

| Port | Usage | Exposition cible |
|---|---|---|
| 80 | Traefik → redirection 301 vers HTTPS | Internet |
| 443 | Traefik → tout le trafic HTTPS | Internet |
| 22 | SSH | **restreint** à des adresses connues, jamais grand ouvert |
| 4317 / 4318 | OTLP (gRPC / HTTP) | **interne uniquement** — aujourd'hui publiés, à fermer |
| 8080 | UI SigNoz | via Traefik + authentification uniquement |
| 3000 | interface web de Dozzle | via Traefik + authentification |
| 3001 | Uptime Kuma | via Traefik (+ ouverture publique si page de statut cliente) |
| 3000 / 5432 | Umami / PostgreSQL | **interne uniquement** |
| 9898 | Backrest | via Traefik + authentification |
| 20128 | omniroute | déjà en place, inchangé pour l'instant |

**Règle** : un service exposé passe **par Traefik en HTTPS**, jamais par un port d'hôte publié. Les ports d'hôte trouvés sur `signoz-f599` et `omniroute-lnlo` sont une dette à résorber, pas un modèle.

## 4. Volumes

| Volume | Service | Contenu | Sauvegardé |
|---|---|---|---|
| `signoz-clickhouse` | ClickHouse | traces, métriques, journaux | oui (cœur de l'observabilité) |
| `signoz-sqlite` | SigNoz | tableaux de bord, alertes, utilisateurs | **oui, prioritaire** (petit et irremplaçable) |
| `signoz-user-scripts` | ClickHouse | fonctions utilisateur | oui |
| `traefik-letsencrypt` | Traefik | `acme.json` : tous les certificats | **oui, prioritaire** |
| `uptime-kuma-data` | Uptime Kuma | moniteurs, historique, pages de statut | oui |
| `backrest-config` + dépôt local | Backrest | configuration, cache Restic | oui |
| `umami-db` | PostgreSQL | statistiques | oui |
| `client-<x>_db` | PostgreSQL | données du client | **oui** |
| `client-<x>_uploads` | application | médias déposés | **oui** |
| `hermes-agent-wezi` (bind `./data`) | Hermes | vault Obsidian, sessions, secrets | **oui, critique** |

> [!warning] Un volume non vérifié n'est pas un volume
> L'inventaire réel des volumes n'a pas pu être lu (pas de socket Docker). À confirmer avec `docker volume ls` et `du -sh` via `scripts/audit-hote.sh` **avant** d'écrire la politique de sauvegarde — sinon on sauvegarde à l'aveugle.

## 5. Réseaux Docker

| Réseau | Rôle | Qui s'y branche |
|---|---|---|
| `proxy` | point d'entrée | Traefik + tout service exposé portant des étiquettes Traefik |
| `internal-<client>` | données du client | l'application du client + **sa** base de données |
| `observability` | collecte | SigNoz (collecteur) + les collecteurs OTel des applications |
| `backup` | sauvegarde | Backrest + les conteneurs à sauvegarder (base pour les vidages) |

**Aucune base de données n'est jamais branchée sur `proxy`, et aucun port de base n'est publié.**

> [!note] Le mode hôte de Traefik, décision à assumer
> Traefik tourne aujourd'hui en `network_mode: host`, ce qui lui permet d'atteindre n'importe quel conteneur de n'importe quel réseau. C'est fonctionnel et **sans rupture**, mais l'isolation entre clients ne vient donc **pas** de Traefik : elle vient du fait que chaque client a son réseau, sa base et ses identifiants, et que rien de tout cela n'est publié. Recommandation : **garder le mode hôte** au démarrage (changer de mode, c'est risquer le HTTPS qui marche), et documenter la règle. Le passage à un Traefik en réseau `proxy` est une amélioration ultérieure, pas un préalable.

## 6. Ressources estimées

Base mesurée : **2 vCPU / 8 Go / 100 Go**, aujourd'hui **11 % de CPU** et **2,2 Go de RAM en moyenne**, avec un pic déjà observé à **7,7 Go**.

| Composant | RAM au repos | RAM en charge | CPU | Disque |
|---|---|---|---|---|
| Traefik | ~78 Mo (mesuré) | ~150 Mo | négligeable | négligeable |
| SigNoz — ClickHouse | ~775 Mo | 2–3 Go | 15–25 % sous charge | **le poste qui grandit** |
| SigNoz — ZooKeeper | ~775 Mo | ~800 Mo | faible | faible |
| SigNoz — collecteur + cœur | ~85 Mo | ~400 Mo | 5–10 % | ~2,7 Go d'images |
| Collecteur d'hôte OTel | ~60 Mo | ~120 Mo | 1–3 % | négligeable |
| Uptime Kuma | ~120 Mo | ~200 Mo | faible | ~200 Mo |
| Dozzle | ~25 Mo | ~50 Mo | négligeable | négligeable |
| Backrest | ~80 Mo | ~300 Mo pendant un envoi | pics ponctuels | dépôt local + cache |
| Umami + PostgreSQL | ~150 Mo | ~350 Mo | faible | selon trafic |
| Trivy | 0 (outil ponctuel) | 0 | pics en CI | ~1 Go d'images, à nettoyer |
| fail2ban | ~30 Mo | ~40 Mo | négligeable | négligeable |
| **Total à ajouter** | **~500 Mo** | **~1,5 à 2 Go** | | **+ 5 à 10 Go** |

Données de dimensionnement SigNoz (documentation officielle et mesures publiées, consultées le 2026-09-29) : minimum **2 vCPU / 4 Go**, recommandé **4–8 vCPU / 8–16 Go**, « l'essentiel de la mémoire va à ClickHouse » ; mesures réelles : **~1,6 Go au repos**, **~3,4 Go sous charge modérée**, **~4,2 Go de disque par 24 h** à ~1 000 lignes de journal et 100 traces par seconde.

> [!warning] Le dimensionnement est LE sujet de ce plan
> Sur les 8 Go, il ne reste aujourd'hui qu'environ **5,5 Go** de marge, et la machine a déjà été vue à 7,7 Go occupés. Ajouter l'empilement ci-dessus (~1,5 à 2 Go en pointe) sur cette base, c'est accepter un risque d'OOM pendant les pointes de collecte. Deux voies :
> - **Voie A — passer en KVM 4 (4 vCPU / 16 Go)** [recommandée] : le plan complet tient sans plafonds douloureux, et la plateforme reste saine jusqu'à 10–20 sites. C'est le seul vrai coût du projet, et ce n'est pas une licence.
> - **Voie B — rester en KVM 2** : faisable, à condition de plafonner ClickHouse (2 Go), de fixer une rétention courte, d'exclure les journaux volumineux de l'ingestion SigNoz, et de surveiller la mémoire comme un moniteur d'Uptime Kuma à part entière.
>
> **Ce choix conditionne tout le reste : il faut le trancher avant d'installer.**

## 7. Risques et conflits

### 7.1 Dokploy — conflit frontal, recommandation : ne pas l'installer

**Dokploy embarque et pilote son propre Traefik**, et **n'offre aucune option pour utiliser un reverse-proxy existant ou le désactiver** : la demande de fonctionnalité est ouverte et non implémentée (Dokploy/dokploy#4307), et la limitation est documentée par des tiers (« It's Traefik or nothing »). Or sur cette VM, **Traefik occupe déjà 80 et 443 en mode hôte** et sert des certificats Let's Encrypt valides. Installer Dokploy revient à mettre deux reverse-proxies sur les mêmes ports.

À cela s'ajoutent : ~2 Go de RAM et un PostgreSQL + Redis supplémentaires sur une machine déjà à 2,2 Go de moyenne ; un second plan de contrôle à sécuriser ; et un accès complet au socket Docker.

**La fonction de déploiement est déjà couverte** par le Docker Manager Hostinger (dépôt d'un `docker-compose.yml` par projet, API REST, interface hPanel, journaux, cycle de vie). C'est lui qui a déployé les dix projets existants.

| Option | Conséquence |
|---|---|
| **Garder le Docker Manager** [recommandé] | gratuit, déjà en place, aucune rupture du HTTPS |
| Dokploy seul | il faut libérer 80/443, donc **perdre le HTTPS et le routage actuels** — régression |
| Dokploy + Traefik existant | non supporté, les deux se disputeront les ports |

### 7.2 Les autres conflits et risques identifiés

| Risque | Détail | Traitement |
|---|---|---|
| **UI SigNoz publique en clair** | port 32859, HTTP 200, sans authentification | fermer le port d'hôte, passer par Traefik + authentification |
| **Aucun pare-feu** | ni Hostinger, ni preuve de `ufw` | créer les règles **puis** activer, puis synchroniser |
| **Mot de passe en clair** | dans le `docker-compose.yml` du projet `n8n` | régénérer et déplacer vers l'environnement du projet |
| **Ports OTLP publiés** | 4317/4318 annoncés sur `0.0.0.0` | à basculer en réseau interne |
| **Zitadel recréé pendant l'audit** | `zitadel-hz1z` → `zitadel-vxri`, nouveaux ports | **qui agit sur le serveur ?** à clarifier avant d'écrire quoi que ce soit |
| **Identifiants de projet instables** | le nom change à chaque recréation (suffixe aléatoire) | la convention de nommage doit porter sur les **journaux/étiquettes**, pas sur le nom du projet |
| **Images `latest` partout** | traefik, signoz, otel-collector, zitadel, omniroute | épingler les versions pour que « ça marchait hier » soit reproductible |
| **Aucune rétention ClickHouse** | 100 Go, disque inconnu, collecte qui grossit | TTL + alarme disque |
| **Réplication ClickHouse sur un nœud** | ZooKeeper + `REPLICATION=true` | à retirer : c'est du coût sans bénéfice |
| **Réseaux et volumes inconnus** | pas de socket Docker | script hôte avant toute écriture |
| **Adresse ACME non valide** | `@srv1398132.hstgr.cloud` | mettre une adresse réelle (expirations de certificat) |
| **Hermes tourne sur cette VM** | l'agent lui-même est un conteneur de la machine | toute action lourde sur le moteur Docker ou le disque affecte l'agent ; à ne pas oublier dans l'ordre des opérations |
| **Trivy sur un hôte partagé** | scanner des images consomme CPU et disque | le mettre en CI d'abord, sur l'hôte en tâche planifiée et limitée ensuite |

## 8. Ce que contient la cible, brique par brique

### Traefik — durcissement

Ajouts à la configuration existante : journal d'accès au format JSON, **métriques Prometheus** (compteurs 2xx/4xx/5xx, latence, débit — c'est exactement l'exigence §9), **traces OTLP** vers SigNoz, version épinglée, en-têtes de sécurité (HSTS, `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`), limitation de débit sur les points d'entrée, authentification devant chaque console d'administration.

### SigNoz — socle unique d'observabilité

Conformément à l'exigence : **ni Grafana, ni Prometheus, ni Loki, ni Tempo séparés**. SigNoz porte tout. À corriger : plafonds mémoire par conteneur, rétention TTL (15 à 30 jours pour commencer), suppression de la réplication, fermeture des ports OTLP, authentification de l'UI.

Convention d'étiquettes OTel pour distinguer les clients, appliquée à **chaque** ressource émise :

```
client=<slug>            ex. client-a
project=<slug>           ex. website-a
environment=production
service=<nom>
version=<tag-de-l-image>
```

Les tableaux de bord SigNoz se construisent ensuite par regroupement sur `client` et `project` — c'est ce qui rendra le multi-client lisible sans reconstruire la brique d'observabilité à chaque nouveau site.

### Disponibilité, journaux, sauvegarde, sécurité, statistiques

- **Uptime Kuma** : surveillance HTTP de chaque site et de chaque point de santé d'API (`/health`), intervalle 60 s, délai 10 s, 3 tentatives, alertes. Une page de statut par client (fonction native). Cloisonnement : ne jamais mettre les moniteurs de deux clients sur la même page publique.
- **Dozzle** : lecture des journaux par conteneur, derrière authentification. Rôle strictement distinct de SigNoz — dépannage rapide contre observabilité. Il lui faut le socket Docker en **lecture seule**.
- **Backrest + Restic** : dépôt **chiffré** (la phrase de passe ne doit figurer nulle part dans un fichier compose ni dans le dépôt Git), planification automatique, politique de rétention (par exemple 7 quotidiennes / 4 hebdomadaires / 6 mensuelles), vérification périodique `restic check --read-data-subset`, et **un test de restauration réel consigné** — un envoi non testé ne vaut pas une sauvegarde. Le stockage distant reste à choisir (§9).
- **Trivy** : `aquasecurity/trivy-action` dans GitHub Actions (gratuit pour les dépôts publics, quota inclus pour les privés) avec `exit-code: 1` sur les vulnérabilités critiques **et** les secrets détectés ; plus une analyse planifiée sur l'hôte. Un déploiement dont l'image ne passe pas le seuil ne part pas.
- **Umami** : une identité de site par client, statistiques strictement séparées. Nécessite PostgreSQL (version 2 de l'application) — **sa propre base**, jamais celle d'un client ni celle de Zitadel.

## 9. Décisions à trancher avant d'écrire

| # | Question | Options |
|---|---|---|
| 1 | **Qui a lancé les 11 opérations Docker du 29/09, dont le `down`/`up` de 12 h 03 ?** | toi / un autre agent / une tâche planifiée — il faut un interlocuteur unique sur cette machine |
| 2 | **Dimensionnement** | **KVM 4 (4 vCPU / 16 Go)** [recommandé] ou KVM 2 avec plafonds stricts |
| 3 | **Domaine** | rester sur `<service>.srv1398132.hstgr.cloud` (fonctionne, 0 €) / activer un domaine gratuit Hostinger / brancher un domaine que tu possèdes déjà |
| 4 | **Stockage distant des sauvegardes** | SFTP vers ton PC ou un NAS (gratuit) / second disque sur ce VPS / stockage objet — aucun dépôt distant n'existe aujourd'hui |
| 5 | **Dokploy** | renoncer [recommandé] / le vouloir malgré le conflit Traefik (à ce moment-là : VM dédiée, ou arrêt du Traefik actuel avec perte du HTTPS en place) |
| 6 | **Les 5 projets arrêtés** (`n8n`, `maxun-36c6`, `paperclip-k5qu`, `docker-registry`, `agent-zero-qhh9`) | à conserver, à redémarrer, ou à supprimer ? **Rien ne sera supprimé sans ton accord explicite.** |
| 7 | **Zitadel** | le réutiliser comme fournisseur d'identité pour les consoles d'admin / le laisser en l'état |

## 10. Ordre d'exécution proposé (après validation)

Chaque étape se termine par une vérification réelle : état du service, journaux, ports, volumes, connectivité — et un compte rendu. En cas d'erreur : cause expliquée, correction proposée, **aucune donnée supprimée sans accord**.

1. **Diagnostic hôte** — `scripts/audit-hote.sh` (lecture seule), pour combler les zones d'ombre de §4 et §5.
2. **Pare-feu Hostinger** — règles accept (SSH restreint, 80, 443) créées **avant** activation, puis synchronisation. Vérification depuis l'extérieur.
3. **Sécurité immédiate** — fermer les ports d'hôte de l'UI SigNoz et de l'omniroute, régénérer le mot de passe en clair de `n8n`, fail2ban.
4. **Durcissement Traefik** — journal d'accès, métriques, traces, version épinglée, en-têtes, authentification des consoles.
5. **SigNoz** — plafonds, rétention, suppression de la réplication, authentification, tableaux de bord par client.
6. **Collecteur d'hôte OTel** — CPU, RAM, disque, réseau, conteneurs.
7. **Uptime Kuma** — sites, points de santé, pages de statut.
8. **Dozzle** — journaux, derrière authentification.
9. **Umami** — base dédiée, un site par client.
10. **Backrest + Restic** — dépôt distant, planification, rétention, puis **test de restauration réel**.
11. **Trivy** — workflow GitHub Actions avec seuil bloquant + analyse planifiée hôte.
12. **Modèle client** — `docker-compose` réutilisable, script d'ajout en une commande.
13. **Documentation** — les sept documents listés au §11.
14. **Réseaux et volumes** — mise en conformité des projets existants (à faire par petits pas, un projet à la fois, avec vérification après chacun).

## 11. Documentation à produire

`ARCHITECTURE.md` · `INSTALLATION.md` · `BACKUP.md` · `SECURITY.md` · `MONITORING.md` · `DISASTER-RECOVERY.md` · `CLIENT-ONBOARDING.md`

Contenu, sans jamais y mettre une valeur de secret : architecture · ports · réseaux · volumes · emplacement des identifiants (jamais les valeurs) · DNS · certificats · sauvegarde et restauration · surveillance · ajout d'un client · retrait d'un client · reprise après incident.

## 12. Procédure d'ajout d'un client (cible)

```
Nouveau client
   |
   +-- 1. Domaine          -> décision : technique Hostinger ou domaine du client
   +-- 2. Base de donnees  -> PostgreSQL dedie, reseau internal-<client>, aucun port publie
   +-- 3. Projet Compose   -> copie du modele, variables remplacees (nom, domaine, base)
   +-- 4. Route Traefik    -> etiquettes du modele, HTTPS Let's Encrypt automatique
   +-- 5. Verification     -> 200 en HTTPS, certificat valide, redirection 301, base injoignable de l'exterieur
   +-- 6. Surveillance     -> moniteur Uptime Kuma + point de sante d'API
   +-- 7. Observabilite    -> etiquettes OTel client/project/environment/service/version
   +-- 8. Sauvegarde       -> volume et base ajoutes au plan Restic, premiere execution verifiee
   +-- 9. Statistiques     -> site Umami dedie, code de suivi sans cookie
   +-- 10. Securite             -> Trivy sur l'image, donc build avant deploiement
```

Les étapes 3, 4, 6, 7, 8 et 9 doivent tenir dans **un seul fichier de valeurs à remplir** et un script. C'est le critère de réussite de la plateforme : passer de 3 à 20 sites sans reconstruire quoi que ce soit.

## Sources

- API Hostinger — interrogation du 2026-09-29, détail dans [[Infrastructure — Audit du 2026-09-29]].
- Dokploy — option d'utiliser un reverse-proxy existant : `github.com/Dokploy/dokploy/discussions/4307` (demande ouverte, non implémentée), consulté le 2026-09-29. Limitation « Traefik ou rien » rapportée par des sources tierces, non contredite par la documentation officielle.
- SigNoz — planification des ressources : `signoz.io/docs/setup/capacity-planning/community/resources-planning` ; mesures publiées sur VPS : ~1,6 Go au repos, ~3,4 Go sous charge modérée, ~4,2 Go de disque/24 h. Consultés le 2026-09-29.
