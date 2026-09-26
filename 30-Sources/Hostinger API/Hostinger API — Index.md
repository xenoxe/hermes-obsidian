---
tags: [hostinger, api, reference]
---

# Hostinger API — Index

Documentation de référence de l'API publique Hostinger, générée à partir du **spec OpenAPI officiel v1.54.2** (312 chemins, **392 endpoints**, 11 produits) et des pages officielles [docs.hostinger.com](https://docs.hostinger.com/api-reference/overview).

## Les notes

- [[Hostinger API — Auth, quotas et erreurs]] — clé API, limite de débit, pagination, format des erreurs et codes HTTP
- [[Hostinger API — VPS]] — machines virtuelles, Docker Manager, firewall, snapshots, sauvegardes (64 endpoints) + nos recettes de déploiement Traefik
- [[Hostinger API — Domaines et DNS]] — portefeuille, disponibilité, transferts, WHOIS, zone DNS, snapshots DNS (48 endpoints)
- [[Hostinger API — Mail]] — boîtes, alias, redirections, répondeurs, webhooks, journaux (38 endpoints)
- [[Hostinger API — Hébergement web et WordPress]] — hébergement mutualisé, Agency, WordPress, Horizons (151 endpoints)
- [[Hostinger API — Billing, Reach et Ecommerce]] — facturation, marketing Reach, boutiques (90 endpoints)
- [[Hostinger API — Référence exhaustive]] — les 392 endpoints en tableaux compacts, groupés par produit (la carte complète, à fouiller avec Ctrl+F)

## En deux lignes

- **Base URL** : `https://developers.hostinger.com` (⚠️ `api.hostinger.com` renvoie une erreur Cloudflare 530/1016 — ne pas l'utiliser)
- **Auth** : `Authorization: Bearer <token>` + `Content-Type: application/json`
- **Version** : chaque chemin porte sa version (`/api/vps/v1/...`, `/api/hosting/v1/...`)
- **Quota** : 90 requêtes/minute par utilisateur (429 au-delà)

## Les produits couverts

| Produit | Préfixe | Endpoints |
|---|---|---|
| Hébergement web | `/api/hosting/v1` | 105 |
| VPS | `/api/vps/v1` | 64 |
| Marketing email (Reach) | `/api/reach/v1` | 52 |
| Domaines | `/api/domains/v1` | 40 |
| Hébergement agence | `/api/agency-hosting/v1` | 40 |
| Email / boîtes mail | `/api/mail/v1` | 38 |
| Ecommerce | `/api/ecommerce/v1` | 29 |
| Facturation | `/api/billing/v1` | 9 |
| DNS | `/api/dns/v1` | 8 |
| Horizons (sites IA) | `/api/horizons/v1` | 6 |
| Direct / divers | `/api/v2/direct` | 1 |

## Outils officiels

| Outil | Installation | Pour quoi |
|---|---|---|
| CLI Hostinger | `brew install hostinger/tap/hostinger` | Terminal, scripts shell, CI |
| Python SDK | `pip install hostinger-api` | Applications Python |
| TypeScript SDK | `npm install @hostinger/sdk` | Node / navigateur |
| PHP SDK | `composer require hostinger/api-php-sdk` | Applications PHP |
| Serveur MCP | `npx @hostinger/mcp` | Assistants IA |
| Spec OpenAPI | `https://raw.githubusercontent.com/hostinger/api/main/openapi.json` | Génération de clients, codegen |

Terraform, Ansible, n8n et WHMCS existent aussi — voir la page officielle [Tools & integrations](https://docs.hostinger.com/api-reference/tools).

## Règles de travail sur ce compte

> [!warning] Périmètre
> Déploiement **uniquement** sur `srv1398132.hstgr.cloud` (VM id **1398132**, IP 148.230.114.215).
> **Ne jamais** écrire sur `srv788682.hstgr.cloud` (VM id **788682**) — machine hors périmètre.

- **HTTPS obligatoire via Traefik**, jamais de port hôte publié (`host:container` interdit) : on route dans `websecure` (443).
- **Préférer une image officielle** au catalogue avant d'écrire un compose maison.
- Toute opération d'écriture : d'abord relire la ressource (`GET`), puis décider.

## Pièges connus

| Piège | Symptôme | Solution |
|---|---|---|
| Mauvais domaine API | Cloudflare 530 / error 1016 | Utiliser `developers.hostinger.com`, jamais `api.hostinger.com` |
| User-Agent de script | HTTP 403 / error 1010 (Cloudflare) | Envoyer `curl/8.7.1` ou un UA navigateur |
| Opérations asynchrones | `200/202` mais rien ne change tout de suite | Poller la ressource (`/containers`, `/actions`) au lieu de renvoyer la requête |
| Création de projet Docker | Le projet existant est remplacé | `POST .../docker` remplace par nom — c'est voulu |
| Quota | `429 Too Many Requests` | Max 90 req/min ; lire `X-RateLimit-Limit` / `X-RateLimit-Remaining` |
| Champ `Host` de Traefik v3 | 404 + certificat par défaut | Backticker le domaine littéral dans la règle : `Host(\`app.exemple.com\`)` |

## Rafraîchir cette documentation

```bash
# 1. Récupérer le spec officiel (à rouvrir pour voir le nouveau chemin)
curl -s https://raw.githubusercontent.com/hostinger/api/main/openapi.json -o /tmp/openapi.json
# 2. Pages de prose (GitBook sert du markdown en ajoutant .md à l'URL)
curl -s https://docs.hostinger.com/api-reference/overview.md
```

Dernière génération : à partir du spec **v1.54.2**.

---

*Voir aussi : [[Hostinger API — Auth, quotas et erreurs]] · [[Hostinger API — VPS]] · [[Hostinger API — Référence exhaustive]]*
