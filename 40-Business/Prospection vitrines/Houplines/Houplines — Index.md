---
tags: [prospection, vitrine, index, houplines]
ville: Houplines
statut_global: en cours
prospects_total: 12
prospects_contactes: 0
date_creation: 2026-09-29
date_maj: 2026-09-29
verifie_le: 2026-09-29
---

# Houplines — Index

Retour : [[Prospection vitrines — Index]] · Dossiers : [[La Tradition — Fiche]]

Prospection des commerces de **Houplines** (59116). Commune de résidence de Moh Amed (Nord, près de Lille) ; 12 candidats relevés sans site déclaré dans OSM. Campagne ouverte le 2026-09-29, dernier passage le 2026-09-29.

## Le tableau des prospects

| Commerce | Activité | Statut | Dernière action | Date | Prochain pas | Dossier |
|---|---|---|---|---|---|---|
| La Tradition | Boulangerie-pâtisserie | dossier_prêt | fiche + mail + prompt rédigés | 2026-09-29 | premier contact | [[La Tradition — Fiche]] |

Statuts autorisés : `nouveau` · `vérifié` · `dossier_prêt` · `contacté` · `relancé` · `réponse` · `gagné` · `perdu`.

## Compteurs

| Étape | Nombre |
|---|---|
| Commerces repérés (OSM, sans site déclaré) | 12 |
| Dossiers écrits (`dossier_prêt`) | 1 |
| Contactés (mail ou appel) | 0 |
| Relancés | 0 |
| Réponses reçues | 0 |
| Sites en test mis en ligne | 0 |
| Gagnés | 0 |
| Perdus | 0 |

## Commande de repérage utilisée

```bash
python3 /opt/data/profiles/prospect-web/skills/prospection/prospection-locale/scripts/osm_prospects.py city "Houplines" --out /opt/data/cache/scratch/prospect-demo/houplines.json
```

Requête Overpass : `node["shop"|"amenity"|"craft"|"office"]["name"][!website][!"contact:website"]` sur la zone administrative — puis **vérification obligatoire** (recherche web + contrôle des domaines évidents) avant de retenir un prospect.

## File d'attente (candidats non traités, par score décroissant)

| Commerce | Activité (OSM) | Score | Tél. | Page sociale | Statut |
|---|---|---|---|---|---|
| Sauvage Peinture | painter | 3 | — | — | à vérifier |
| Friterie | fast_food | 2 | — | — | à vérifier |
| Concessionnaire Poids Lourds Dubreu | truck | 0 | — | — | à vérifier |
| Toitures Partenaires | roofer | 0 | — | — | à vérifier |
| Le Reinitas Café-Tabac | cafe | 0 | — | — | à vérifier |
| Pompes funèbres Remory | funeral_directors | 0 | — | — | à vérifier |
| Unal Market | convenience | 0 | — | — | à vérifier |
| Mobilauto | car_parts | 0 | — | — | à vérifier |
| DF Auto Service | car | 0 | — | — | à vérifier |
| Au Pain de Campagne | bakery | 0 | — | — | à vérifier |
| Vanomotors | car_repair | 0 | — | — | à vérifier |

## Écartés

- **Toitures Partenaires** (couvreur, Houplines) — **écarté le 2026-09-29** : retenu par OSM sans tag `website`, mais son site `toitures-partenaire.fr` répond en 200 (« Votre expert toiture | Houplines (59) | TOITURES PARTENAIRE »). Le contrôle de domaine a démenti la recherche web, qui ne le remontait pas.

## Ce qui a été fait dans cette ville

- 2026-09-29 — repérage Overpass/OSM : 12 commerces sans tag `website` ; vérification en ligne des candidats à fort score ; 1 dossiers écrits.
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
