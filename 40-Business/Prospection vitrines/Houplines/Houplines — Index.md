---
tags: [prospection, vitrine, index, houplines]
ville: Houplines
statut_global: en cours
prospects_total: 18
prospects_contactes: 0
date_creation: 2026-09-29
date_maj: 2026-09-29
verifie_le: 2026-09-29
---

# Houplines — Index

Retour : [[Prospection vitrines — Index]] · Dossiers : [[La Tradition — Fiche]] · [[Sauvage Peinture — Fiche]] · [[DF Auto Services — Fiche]]

Prospection des commerces de **Houplines** (59116). Commune de résidence de Moh Amed (Nord, près de Lille). Campagne ouverte le 2026-09-29, dernier passage le 2026-09-29.

## Le tableau des prospects

| Commerce | Activité | Statut | Dernière action | Date | Prochain pas | Dossier |
|---|---|---|---|---|---|---|
| La Tradition | Boulangerie-pâtisserie | dossier_prêt | fiche + mail + prompt rédigés | 2026-09-29 | premier contact | [[La Tradition — Fiche]] |
| Sauvage Peinture | Peintre en bâtiment (travaux de peinture et vitrerie) | dossier_prêt | fiche + mail + prompt rédigés | 2026-09-29 | premier contact | [[Sauvage Peinture — Fiche]] |
| DF Auto Services | Garage et négoce de véhicules (commerce de voitures et véhicules légers) | dossier_prêt | fiche + mail + prompt rédigés | 2026-09-29 | premier contact | [[DF Auto Services — Fiche]] |

Statuts autorisés : `nouveau` · `vérifié` · `dossier_prêt` · `contacté` · `relancé` · `réponse` · `gagné` · `perdu`.

## Compteurs

| Étape | Nombre |
|---|---|
| Commerces repérés (OSM, sans site déclaré) | 18 |
| Dossiers écrits (`dossier_prêt`) | 3 |
| Contactés (mail ou appel) | 0 |
| Relancés | 0 |
| Réponses reçues | 0 |
| Sites en test mis en ligne | 0 |
| Gagnés | 0 |
| Perdus | 0 |

## Commande de repérage utilisée

```bash
python3 /opt/data/profiles/prospect-web/skills/prospection/prospection-locale/scripts/osm_prospects.py city "Houplines" --limit 250 --categories shop,amenity,craft,office --out /tmp/houplines.json
```

Requête Overpass : POI `shop` / `amenity` / `craft` avec `[!website][!"contact:website"]`, filtrée des chaînes, puis **vérification obligatoire** (recherche web **et** contrôle des domaines évidents) avant de retenir un prospect.

## Écartés (avec la preuve)

- **Toitures Partenaires** — **écarté le 2026-09-29** : site existant (`toitures-partenaire.fr`, réponse 200, titre « Votre expert toiture | Houplines (59) | TOITURES PARTENAIRE »), que la recherche web ne remontait pas.
- **Chez les Jo's** (friterie) — **écarté le 2026-09-29** : fiche cartographique indiquant « ne fonctionne plus » (Yandex Maps, consulté le 2026-09-29).
- **Banette** (boulangerie, 121 rue… relevé OSM) — **écarté** : enseigne d'un réseau national de boulangeries franchisées, hors cible.
- **Au Pain de Campagne** et **Le Reinitas Café-Tabac** — **en attente** : aucun site trouvé, mais aucune coordonnée exploitable (ni téléphone ni page publique) : à compléter sur place avant démarchage.

## Ce qui a été fait dans cette ville

- 2026-09-29 — repérage Overpass/OSM : 18 commerces sans tag `website` ; vérification des meilleurs candidats ; 3 dossiers écrits.

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
