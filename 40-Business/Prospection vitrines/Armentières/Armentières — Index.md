---
tags: [prospection, vitrine, index, armentières]
ville: Armentières
statut_global: en cours
prospects_total: 137
prospects_contactes: 0
date_creation: 2026-09-29
date_maj: 2026-09-29
verifie_le: 2026-09-29
---

# Armentières — Index

Retour : [[Prospection vitrines — Index]] · Dossiers : [[Fontana Fleurs — Fiche]] · [[La Prairie — Fiche]] · [[Seduc'tif by Justine — Fiche]]

Prospection des commerces de **Armentières** (59280). Commune voisine de Houplines, centre-ville commerçant (Nord). Campagne ouverte le 2026-09-29, dernier passage le 2026-09-29.

## Le tableau des prospects

| Commerce | Activité | Statut | Dernière action | Date | Prochain pas | Dossier |
|---|---|---|---|---|---|---|
| Fontana Fleurs | Fleuriste | dossier_prêt | fiche + mail + prompt rédigés | 2026-09-29 | premier contact | [[Fontana Fleurs — Fiche]] |
| La Prairie | Épicerie fine / fromagerie | dossier_prêt | fiche + mail + prompt rédigés | 2026-09-29 | premier contact | [[La Prairie — Fiche]] |
| Seduc'tif by Justine | Salon de coiffure | dossier_prêt | fiche + mail + prompt rédigés | 2026-09-29 | premier contact | [[Seduc'tif by Justine — Fiche]] |

Statuts autorisés : `nouveau` · `vérifié` · `dossier_prêt` · `contacté` · `relancé` · `réponse` · `gagné` · `perdu`.

## Compteurs

| Étape | Nombre |
|---|---|
| Commerces repérés (OSM, sans site déclaré) | 137 |
| Dossiers écrits (`dossier_prêt`) | 3 |
| Contactés (mail ou appel) | 0 |
| Relancés | 0 |
| Réponses reçues | 0 |
| Sites en test mis en ligne | 0 |
| Gagnés | 0 |
| Perdus | 0 |

## Commande de repérage utilisée

```bash
python3 /opt/data/profiles/prospect-web/skills/prospection/prospection-locale/scripts/osm_prospects.py city "Armentières" --limit 250 --categories shop,amenity,craft,office --out /tmp/armentières.json
```

Requête Overpass : POI `shop` / `amenity` / `craft` avec `[!website][!"contact:website"]`, filtrée des chaînes, puis **vérification obligatoire** (recherche web **et** contrôle des domaines évidents) avant de retenir un prospect.

## Écartés (avec la preuve)

- **Poissonnerie Havetz** — **écarté le 2026-09-29** : retenu sans tag `website` dans OSM, mais son site existe et répond : https://www.poissonnerie-havetz-armentieres.fr/ (page « Poissonnerie Havetz Armentières – A l'Huitrière »).
- **Boucherie Le Sanglier** — **écarté le 2026-09-29** : la presse locale documente une fermeture (La Voix du Nord, 05/12/2024 : « le rideau se baissera définitivement ») puis une reprise incertaine (18/05/2026). Commerce à ne pas démarcher sans vérification sur place.

## Ce qui a été fait dans cette ville

- 2026-09-29 — repérage Overpass/OSM : 137 commerces sans tag `website` ; vérification des meilleurs candidats ; 3 dossiers écrits.

## À faire ensuite

1. Vérifier les candidats suivants de la file (les JSON bruts sont dans `/opt/data/cache/scratch/prospect-demo/`).
2. Premier contact — aucun email public trouvé pour ces commerces : téléphone ou passage sur place.
3. Obtenir du client logo, photos et mentions légales (kit d'intake du skill `identite-visuelle`).

## Sources

- https://overpass-api.de/api/interpreter — requête du 2026-09-29 (commerces sans site déclaré dans OSM).
- /opt/data/obsidian-vault/AGENTS.md — conventions du vault, consultées le 2026-09-29.
- Sources détaillées par prospect : section « Sources » de chaque fiche.

## Règle de tenue

- Toute action sur un prospect met à jour le même jour : la fiche, ce tableau, les compteurs, [[Prospection vitrines — Index]].
- Un compteur se recalcule à partir du tableau ; il ne s'estime pas.
