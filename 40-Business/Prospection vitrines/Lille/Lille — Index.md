---
tags: [prospection, vitrine, index, lille]
ville: Lille
statut_global: en cours
prospects_total: 286
prospects_contactes: 0
date_creation: 2026-09-29
date_maj: 2026-09-29
verifie_le: 2026-09-29
---

# Lille — Index

Retour : [[Prospection vitrines — Index]] · Dossiers : [[Friterie Ch'timi — Fiche]] · [[Les Chineurs — Fiche]]

Prospection des commerces de **Lille** (59000/59160). Préfecture du Nord ; 286 candidats relevés sans site déclaré dans OSM, dont 55 avec une page Facebook ou Instagram — la meilleure cible. Campagne ouverte le 2026-09-29, dernier passage le 2026-09-29.

## Le tableau des prospects

| Commerce | Activité | Statut | Dernière action | Date | Prochain pas | Dossier |
|---|---|---|---|---|---|---|
| Friterie Ch'timi | Friterie (restauration rapide sur place et à emporter) | dossier_prêt | fiche + mail + prompt rédigés | 2026-09-29 | premier contact | [[Friterie Ch'timi — Fiche]] |
| Les Chineurs | Bar (Vieux-Lille) — bières, cocktails, tapas, jeux de société | dossier_prêt | fiche + mail + prompt rédigés | 2026-09-29 | premier contact | [[Les Chineurs — Fiche]] |

Statuts autorisés : `nouveau` · `vérifié` · `dossier_prêt` · `contacté` · `relancé` · `réponse` · `gagné` · `perdu`.

## Compteurs

| Étape | Nombre |
|---|---|
| Commerces repérés (OSM, sans site déclaré) | 286 |
| Dossiers écrits (`dossier_prêt`) | 2 |
| Contactés (mail ou appel) | 0 |
| Relancés | 0 |
| Réponses reçues | 0 |
| Sites en test mis en ligne | 0 |
| Gagnés | 0 |
| Perdus | 0 |

## Commande de repérage utilisée

```bash
python3 /opt/data/profiles/prospect-web/skills/prospection/prospection-locale/scripts/osm_prospects.py city "Lille" --out /opt/data/cache/scratch/prospect-demo/lille.json
```

Requête Overpass : `node["shop"|"amenity"|"craft"|"office"]["name"][!website][!"contact:website"]` sur la zone administrative — puis **vérification obligatoire** (recherche web + contrôle des domaines évidents) avant de retenir un prospect.

## File d'attente (candidats non traités, par score décroissant)

| Commerce | Activité (OSM) | Score | Tél. | Page sociale | Statut |
|---|---|---|---|---|---|
| Zythum | bar | 9 | +33 3 62 65 72 38 | FB | à vérifier |
| Le B-Routh | restaurant | 9 | +33 320421020 | FB | à vérifier |
| Het Hol (fermé) | restaurant | 8 | +33 320 293 011 | FB | à vérifier |
| Pub Goudale Restaurant Lomme | restaurant | 7 | +33 3 20 08 03 81 | FB | à vérifier |
| Café Bellot | cafe | 7 | — | FB | à vérifier |
| Amul solo | bar | 6 | +33 3 20 74 44 90 | FB | à vérifier |
| Le Turenne | cafe | 6 | +33320936632 | — | à vérifier |
| Asie Gambeta | convenience | 6 | +33 7 52 62 38 90 | FB | à vérifier |
| Hill Bar | chocolate | 6 | +33 3 59 54 41 62 | FB | à vérifier |
| Phoenicia Pizzeria | fast_food | 6 | +33 3 20 31 79 73 | FB | à vérifier |

## Écartés

- Aucun écarté pour l'instant : les candidats de score ≥ 7 ont tous été confirmés sans site (Friterie Ch'timi, Les Chineurs, Café Bellot, Zythum).

## Ce qui a été fait dans cette ville

- 2026-09-29 — repérage Overpass/OSM : 286 commerces sans tag `website` ; vérification en ligne des candidats à fort score ; 2 dossiers écrits.
- 2026-09-29 — écartés après vérification : voir la section « Écartés ».

## À faire ensuite

1. Vérifier puis traiter les 5 premiers de la file d'attente (recherche web + contrôle de domaine).
2. Premier contact sur les dossiers prêts — canal à trancher : aucun email public trouvé pour ces commerces (téléphone ou passage sur place).
3. Obtenir du client le logo, les photos et les mentions légales (kit d'intake du skill `identite-visuelle`).

## Sources

- https://overpass-api.de/api/interpreter — requête du 2026-09-29 (commerces sans site déclaré dans OSM).
- /opt/data/obsidian-vault/AGENTS.md — conventions du vault, consultées le 2026-09-29.
- Sources détaillées par prospect : voir la section « Sources » de chaque fiche.

## Règle de tenue

- Toute action sur un prospect met à jour le même jour : la fiche, ce tableau, les compteurs, [[Prospection vitrines — Index]].
- Un compteur se recalcule à partir du tableau ; il ne s'estime pas.
