---
tags: [pfe, reseau, dns, hls, twitch, youtube, publicite]
---

# PFE AdShield — Architecture et limites

Note technique du projet (voir [[PFE AdShield — Index]] pour le cadre et l'état). Vérifié le **2026-09-27** sur sources primaires (dépôts et wiki des projets concernés), comme indiqué.

## 1. Le fait qui structure tout le projet

La publicité moderne ne passe plus par des domaines tiers : **YouTube et Twitch insèrent la pub dans la vidéo elle-même, côté serveur** (SSAI, *server-side ad insertion*).

- Chez Twitch, la pub est cousue dans le flux HLS : segment pub et segment de contenu arrivent de la **même URL**, sur `video-weaver.*.hls.ttvnw.net` (`*.playlist.live-video.net`, `*.playlist.ttvnw.net`). Il n'y a donc **aucune requête à bloquer** au niveau réseau. *Vérifié* : le wiki officiel de TTV LOL PRO l'écrit noir sur blanc (« Twitch ads can't be blocked, at all, but can be avoided/bypassed ») et liste les endpoints concernés.
- Conséquence directe : un bloqueur DNS/hosts — l'approche retenue pour ce PFE — **ne peut pas supprimer les pubs du direct de Twitch**, ni le pré-roll YouTube servi depuis les serveurs de la vidéo.

Ce qui **reste** joignable par DNS (et que le programme bloque donc réellement) : régies et places de marché publicitaires (`doubleclick.net`, `googlesyndication.com`, `adnxs.com`, `criteo.com`…), télémétrie pub de Twitch (`ads.twitch.tv`, `spade.twitch.tv`, `edge.ads.twitch.tv`, `analytics.m7g.twitch.tv`), lecteurs publicitaires (`imasdk.googleapis.com`), plus bannières et habillages des sites. Sur les appareils mobiles, TV et consoles — qui n'ont pas d'extension possible — c'est **la seule couche qui fonctionne**.

> Point de rédaction pour le mémoire : c'est une **limite démontrée**, pas un échec. Le protocole de mesure doit la chiffrer (ex. : sur un direct Twitch, X % des requêtes bloquées mais 0 % des créneaux pub supprimés, alors que sur YouTube web la même configuration supprime les bannières et les formats vendus à part).

## 2. Méthodes qui fonctionnent réellement sur Twitch (état 2026, si on ajoute un module)

1. **Interception du manifeste HLS dans le navigateur.** On hook le Web Worker du player, on parse le `.m3u8`, on détecte les marqueurs de pub (`stitched-ad`, `EXT-X-CUE-OUT`, `EXT-X-DATERANGE:CLASS="twitch-stitched-ad"` / `"twitch-maf-ad"`, URLs `/adsquared/`, `/_404/`, `/processing`) et on bascule sur un flux de secours demandé avec un `PlaybackAccessToken` GQL en `playerType=embed|popout|autoplay`. Repli : suppression des segments pub (lecture en pause pendant le créneau). *Vérifié* : liste des marqueurs dans les commits du fork actif `ryanbr/TwitchAdSolutions`, architecture détaillée dans `pixeltris/TwitchAdSolutions`.
   - Fragilité : le `sha256Hash` du token GQL est codé en dur dans les clients → Twitch peut tout casser par un simple changement serveur (fenêtre de panne de 24-72 h selon les projets).
   - Contrainte navigateur : **Manifest V3** (Chrome/Edge) ne permet plus l'injection de scriptlets nécessaire → **Firefox** avec uBlock Origin complet reste la plateforme fiable.
2. **Proxy de playlists** (approche TTV LOL PRO, ~200 k utilisateurs, la plus robuste) : on ne proxifie que les endpoints de playlist (`usher.ttvnw.net`, `*.playlist.*`, `gql.twitch.tv`), **jamais la vidéo** → bande passante négligeable. Le correctif se fait côté serveur, donc l'extension ne casse pas à chaque rotation de hash.
3. **Ce que le PFE ne fera pas** : contourner un abonnement payant, remplacer Twitch Turbo. À assumer dans le mémoire : ces deux méthodes **violent les CGU de Twitch** (contournement de la monétisation du streamer). Bloqueur réseau, en revanche : usage banal, aucune interdiction légale en France.

*Rapporté (sources secondaires, non re-vérifiées en profondeur)* : les guides 2026 situent la bascule définitive de Twitch vers le SSAI et la fin du projet `TwitchAdSolutions` (archivé en mars 2026, forks actifs depuis).

## 3. Architecture du programme livré

`/opt/data/projects/adshield` — Node 26 + TypeScript, **zéro dépendance d'exécution** (runtime natif : `node:dgram`, `node:net`, `node:sqlite`, `node:test`).

```
client (PC, mobile, TV) ──requête DNS──► AdShield ──► resolver amont (1.1.1.1)
                                          │ 1. allowlist ?  → transmis
                                          │ 2. blocklist ?  → 0.0.0.0 / :: (ou NXDOMAIN)
                                          │ 3. sinon cache, puis relais
                                          └─ stats SQLite + API HTTP + tableau de bord
```

- `src/dns-message.ts` — protocole DNS écrit à la main : décodage de la question (avec compression de pointeurs), construction des réponses, lecture des TTL.
- `src/blocklist.ts` — parsing multi-format (hosts, domaine simple, syntaxe Adblock `||domaine^`), matching domaine + sous-domaines, entrées `*.domaine`, **allowlist prioritaire**.
- `src/server.ts` — serveur **UDP + TCP**, cache TTL, relais amont avec requêtes en vol suivies par ID sortant réécrit, compteurs.
- `src/stats.ts` — journal SQLite (`node:sqlite`), agrégats : totaux, taux de blocage, latence amont moyenne, hits de cache, top domaines, par client.
- `src/api.ts` + `public/index.html` — API HTTP et tableau de bord sans framework (rafraîchi toutes les 5 s).
- `scripts/fetch-lists.ts` — fusion des listes publiques (StevenBlack hosts + OISD) et des listes locales curées (`data/blocklist.extra.txt`).
- `scripts/bench.ts` — vérification + mesure A/B (AdShield vs amont direct) : sert à produire les chiffres du mémoire.

## 4. Chiffres relevés le 2026-09-27 (exécution réelle, depuis le conteneur)

- **127 846 domaines** bloqués après fusion (StevenBlack 74 762 + OISD 56 686 + 26 entrées locales curées).
- **Tests : 16/16** — dont un bout-en-bout sans Internet (upstream factice) : blocage, sous-domaine, exception, cache, stats.
- Blocage confirmé : `doubleclick.net`, `googlesyndication.com`, `scorecardresearch.com`, `ads.twitch.tv`, `spade.twitch.tv` → `0.0.0.0`.
- Non bloqués (indispensable, sinon Twitch et YouTube cassent) : `twitch.tv`, `www.twitch.tv`, `usher.ttvnw.net`, `static-cdn.jtvnw.net`, `youtube.com`, `googlevideo.com`.
- Latence : 6,1 ms en moyenne via AdShield contre 9,7 ms en direct sur le même jeu de domaines (n=14, réseau de datacenter — **mesure à refaire sur le réseau de Moh Amed**, l'écart n'est pas concluant ici).
- Cache : 23,7 ms à froid, 7,2 ms à chaud sur le même nom.

## 5. Plan de mesure à tenir pour le rapport

| indicateur | comment | état |
|---|---|---|
| taux de blocage | `/api/stats` après 24 h de navigation normale | à relever |
| surcout de latence | `scripts/bench.ts` sur le réseau local, 100+ requêtes | partiel |
| taux de faux positifs | liste de sites de référence + `POST /api/allow` | à faire |
| efficacité du cache | `cacheHits / (total − blocked)` | à faire |
| limite SSAI chiffrée | session Twitch + YouTube filmée/annotée (requêtes bloquées vs pubs vues) | à faire |

## 6. À faire ensuite (ordre conseillé)

1. Extension MV3 : filtres réseau (`declarativeNetRequest`) + habillage cosmétique, cible **Firefox** (Chrome MV3 dégrade).
2. Session de mesure sur le réseau de Moh Amed, chiffres dans le tableau du §5.
3. Décider du sort de la partie « pubs in-stream » : limite documentée (§1) ou module HLS (§2), à écrire dans le mémoire dans les deux cas.
4. Démo : router le DNS des appareils vers la machine qui exécute AdShield (mobile, TV, console), tableau de bord affiché pendant la soutenance.

## Sources (vérifiées le 2026-09-27)

- TTV LOL PRO — wiki officiel, « How it works » : https://wiki.cdn-perfprod.com/must-read/how-it-works (endpoints proxifiés, SSAI non bloquable).
- `zGato/ScrewTwitchAds` et `ryanbr/TwitchAdSolutions` : https://github.com/zGato/ScrewTwitchAds · https://github.com/ryanbr/TwitchAdSolutions (liste à jour des marqueurs de pub ; méthode vaft).
- `pixeltris/TwitchAdSolutions` — architecture interne (hook du Web Worker, `processM3U8`, tokens de secours) : https://github.com/pixeltris/TwitchAdSolutions
- StevenBlack hosts : https://github.com/StevenBlack/hosts · OISD : https://small.oisd.nl/domainswild
- *Rapporté* (à confirmer si cité dans le mémoire) : guides 2026 sur l'état du blocage Twitch (Manifest V3, archivage de TwitchAdSolutions en mars 2026).
