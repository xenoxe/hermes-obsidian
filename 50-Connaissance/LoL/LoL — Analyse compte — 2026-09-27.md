---
tags: [lol, league-of-legends, analyse-compte, coaching]
date-verification: 2026-09-27
compte: "x3nøx3#EUW"
patch: "26.19"
---

# LoL — Analyse compte — 2026-09-27

**Compte** : `x3nøx3#EUW` — serveur EUW, niveau 117 · **Rôle** : jungler
**Vérifié le** : 2026-09-27 · **Patch live** : 26.19 (Data Dragon 16.19.1)
Voir aussi : [[LoL — Index]] · [[LoL — Patch 26.19 — impacts]] · [[LoL — Veille patches]]

**Sources**
- `deeplol.gg/summoner/euw/x3nøx3-EUW` (profil + onglet Champions) — consulté le 2026-09-27 au navigateur. op.gg et u.gg sont inaccessibles depuis ce serveur (403 CloudFront / mur Cloudflare, revérifié ce jour) ; **source tierce**, non confirmée par l'API Riot.
- `lolalytics.com/lol/<champion>/build/?lane=jungle` — textes de fiches : *Patch 16.19, Ranked Solo/Duo, GLOBAL*, agrégat **Emerald+**. Le palier Silver n'a pas pu être obtenu ce jour (`?tier=silver_plus` → 404, et le filtre « Silver » de la page ne s'est pas appliqué au navigateur).
- **API Riot non utilisée** : aucune `RIOT_API_KEY` dans `/opt/data/profiles/lol-coach/.env` le 2026-09-27 (`riot_fetch.py probe` → « RIOT_API_KEY absente »). Voir « Ce qui n'a pas pu être mesuré ».

## Où il en est

- **Solo/Duo : Silver 1, 66 LP** — 32 V / 31 D (51 %). Pic de saison : S1-80 LP. **+129 LP sur les 30 derniers jours** : il monte, lentement. Flex : non classé.
- 20 dernières parties Solo/Duo : 10 V – 10 D, KDA 2,40 (8,2 / 6,3 / 7,0).
- 17 dernières parties Solo/Duo recomptées à la main depuis l'historique : 9 V – 8 D, 8,0 / 5,8 / 7,0, durée moyenne 29,2 min.
- 52 % de winrate sur 50 parties classées : il n'est pas bloqué par le volume, il est bloqué par les parties qu'il jette.

## Pool réel (onglet Champions, saison 2026, 50 parties classées)

| Champion | Parties | V-D (WR) | KDA | K / D / A | Morts/partie | CS/m | GD@15 |
|---|---|---|---|---|---|---|---|
| Shyvana | 18 | 10-8 (55,6 %) | 3,23 | 9,2 / 5,0 / 6,9 | **5,0** | 8,4 | +481 |
| Briar | 8 | 5-3 (62,5 %) | 2,36 | 8,3 / 6,3 / 6,5 | 6,3 | 7,1 | +256 |
| Master Yi | 5 | 3-2 (60,0 %) | 2,19 | 10,8 / **8,6** / 8,0 | 8,6 | 7,2 | +587 |
| Warwick | 5 | 2-3 (40,0 %) | 2,06 | 7,8 / 6,8 / 6,2 | 6,8 | 7,6 | +642 |
| Lillia | 4 | 2-2 (50,0 %) | 2,54 | 4,5 / 6,0 / 10,8 | 6,0 | 7,6 | +25 |
| Trundle | 3 | 1-2 (33,3 %) | 2,76 | 7,3 / 5,7 / 8,3 | 5,7 | 7,1 | +705 |
| Nocturne | 3 | 1-2 (33,3 %) | 2,48 | 5,3 / 7,0 / 12,0 | 7,0 | 7,5 | +502 |
| Kha'Zix · Diana · Teemo · Rammus | 1 chacun | — | — | — | 4 à 7 | — | — |
| **Total** | **50** | **26-24 (52,0 %)** | **2,64** | **8,2 / 6,0 / 7,7** | **6,0** | 7,7 | +377 |

Sept champions joués plus d'une fois, onze en tout, sur 50 parties : le pool est trop large pour son rang.

## Défaut n°1 — il meurt trop (le chiffre qui coûte le rank)

- **6,0 morts par partie** sur ses 50 parties classées ; **6,3** sur les 20 dernières ; **5,8** sur les 17 recomptées. Trois mesures, même verdict.
- Repère de coaching : **≤ 4 morts/partie** pour un joueur qui monte. Il est 50 % au-dessus.
- **24 % de ses 17 dernières parties à 8 morts ou plus** (4/17) : ce sont les parties qu'il perd presque seul, en portant un champion fragile.
- Le contraste est net *à l'intérieur* de son pool : Shyvana 5,0 morts (KDA 3,23) contre Master Yi **8,6** et Nocturne 7,0. Le niveau de risque dépend du champion choisi, pas de la partie.
- **Ce n'est ni le farm ni la laning phase** : CS/m 7,7 (repère jungle 5,5–7, il est au-dessus) et GD@15 **+377**, positif. Les morts arrivent après, au moment où il devrait convertir son avance. Cohérent avec les données, **non démontré** : il faut la timeline des parties pour trancher (voir ci-dessous).

## Défaut n°2 — trois champions, pas onze

Aligné sur la lecture du patch 26.19 ([[LoL — Patch 26.19 — impacts]]) :

- **Briar — premier choix** : 62,5 % de winrate perso sur 8 parties, rien n'a bougé sur elle en 26.19 (lolalytics la donne à 52,49 % sur 370 737 parties, tier S, rang 3/79 en All Ranks).
- **Shyvana — filet de sécurité** : 55,6 % sur 18 parties, KDA 3,23, 5,0 morts/partie, 8,4 CS/m. Intacte en 26.19 (52,92 % sur 69 437 parties, A-, rang 24/79 en All Ranks). Sous-estimée par les tier lists, très au-dessus de la moyenne dans *ses* mains : c'est son meilleur atout.
- **Lillia — situationnel** : réponse chiffrée à un Master Yi en face (lolalytics la classe parmi celles qu'il bat le moins), et buffée en 26.19 (armure de base 22 → 24, sommeil de R 2,0 → 2,5 s).
- **À arrêter temporairement** : **Master Yi** (8,6 morts/partie, nerfé sur *Alpha Strike* en 26.19 — la pire combinaison chiffrée de son pool), **Warwick** (40 % sur 5 parties), **Trundle** et **Nocturne** (33 % sur 3). Sous 10 parties un winrate est du bruit : ici c'est le compteur de morts et les changements de patch qui tranchent, pas le pourcentage.

## Contexte méta (patch 26.19, classé Solo/Duo, lolalytics)

- **Emerald+ (fiches champions lolalytics)** : Shyvana 52,9 % / A-tier / rang 17/78 / 39 246 parties ; Briar 53,21 % / A-tier / rang 15/78 / 35 824 parties.
- **All Ranks (relevé de la note patch)** : Briar 52,49 % sur 370 737 parties, Shyvana 52,92 % sur 69 437 parties.
- **Prudence** : les deux fenêtres ne sont pas comparables et lolalytics étiquette ses pages de façon incohérente (patch affiché 16.19 dans les fiches, « 16.11 » ailleurs). Aucun de ces taux n'est **Silver** — le palier de Moh Amed. Les ordres de grandeur servent à choisir entre deux picks proches, pas à décider seuls.
- Ses deux picks de prédilection sont A-tier au patch courant et **Shyvana est très peu bannie (5,85 %)** : elle est jouable presque à chaque partie sans être contestée.

## Build de référence (reconstitué, à confirmer dans une fiche dédiée)

- **Shyvana jungle** — Flash + Châtiment · ordre de compétences **Q → W → E** (Q en 1/4/5/7/9) · cœur **Trinity Force + Spear of Shojin** (variantes Death's Dance / Riftmaker selon l'adversaire) · départ Mosstomper Seedling.
- **Briar jungle** — Flash + Châtiment · ordre de compétences **W → Q → E** (W en 1/4/5/7/9) · cœur **Titanic Hydra + Black Cleaver**, bottes Mercury's Treads · départ Mosstomper Seedling.
- L'ordre exact des objets par emplacement et les taux par objet **n'ont pas pu être extraits proprement** : ils ne sont pas cités ici. À vérifier dans [[LoL — Shyvana — fiche]] / [[LoL — Briar — fiche]] avant de présenter le build comme validé au chiffre près.

## Plan de la semaine

1. **Trois champions au maximum** : Briar (ouverture), Shyvana (filet), Lillia (situationnel contre Master Yi). Rien d'autre en classée pendant 20 parties.
2. **Objectif mesurable : ≤ 5 morts par partie sur les 20 prochaines classées** (base de départ : 5,8–6,0). Palier visé ensuite : 4, le repère de coaching. À remesurer avec `riot_fetch.py review` dès que la clé API est de retour.
3. **Protocole de mesure (5 parties)** : noter pour chaque mort la minute et la cause (1v1 de jungle, gank raté, combat de groupe, après un objectif). La répartition manuelle dit si le défaut est au début ou à la fin de la partie ; l'API le dira directement.

## Ce qui n'a pas pu être mesuré

- **Morts avant 14 min, diff d'or à 14 min vs adversaire de voie, part des dégâts, vision/min, KP, table de maîtrise** : nécessitent l'API Riot. Le script est prêt (`scripts/riot_fetch.py`, skill `lol-riot-api`) mais la clé manque : `RIOT_API_KEY` est **absente** de `/opt/data/profiles/lol-coach/.env`. Une clé de développement se régénère sur <https://developer.riotgames.com> et **expire toutes les 24 h**.
- **Taux spécifiques Silver** : non obtenus (404 sur `?tier=silver_plus`, filtre Silver inopérant au navigateur le 2026-09-27). Les taux cités sont Emerald+ ou All Ranks.
- Le profil deeplol affiche 32 V / 31 D en Solo/Duo quand l'onglet Champions agrège 50 parties (26-24) : ce sont deux fenêtres différentes. Ne pas les additionner.

## Suite

- Remettre la clé API Riot → `riot_fetch.py fetch --count 20 --timeline` puis `review` : répartition des morts, part des dégâts, KP, diff d'or à 14 min.
- Écrire `[[LoL — Shyvana — fiche]]` et `[[LoL — Briar — fiche]]` avec les taux par objet et les matchups.
