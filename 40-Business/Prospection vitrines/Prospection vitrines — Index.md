---
tags: [prospection, vitrine, index, business]
statut_global: en cours
villes_total: 3
prospects_total: 441
date_creation: 2026-09-29
date_maj: 2026-09-29
---

# Prospection vitrines — Index

Activité : proposer à des commerces locaux **sans site web** un site vitrine monté en test, gratuit et sans engagement, facturé seulement s'ils le gardent. Un dossier par commerce, trois notes par dossier : fiche, mail icebreaker, note du prompt du site.

Cet index est la racine de l'activité. Le dossier `40-Business/` n'a pas d'index (à créer seulement avec l'accord de Moh Amed).

## Les villes

| Ville | Prospects repérés | Dossiers prêts | Contactés | Réponses | Gagnés | Dernière action | Index |
|---|---|---|---|---|---|---|---|
| Houplines | 18 | 3 | 0 | 0 | 0 | 2026-09-29 | [[Houplines — Index]] |
| Armentières | 137 | 3 | 0 | 0 | 0 | 2026-09-29 | [[Armentières — Index]] |
| Lille | 286 | 2 | 0 | 0 | 0 | 2026-09-29 | [[Lille — Index]] |

## Compteurs globaux

| Étape | Nombre |
|---|---|
| Villes ouvertes | 3 |
| Commerces repérés (OSM, sans site déclaré) | 441 |
| Dossiers écrits (`dossier_prêt`) | 8 |
| Contactés | 0 |
| Relancés | 0 |
| Réponses reçues | 0 |
| Sites en test en ligne | 0 |
| Gagnés | 0 |
| Perdus | 0 |

Taux de réponse : non calculable (0 contacté).

## Les dossiers

Chaque dossier vit dans `Prospection vitrines/<Ville>/<Nom du commerce>/` et contient exactement trois notes : `<Nom> — Fiche.md`, `<Nom> — Mail icebreaker.md`, `<Nom> — Prompt du site.md`.

- **Houplines** : La Tradition · Sauvage Peinture · DF Auto Services
- **Armentières** : Fontana Fleurs · La Prairie · Seduc'tif by Justine
- **Lille** : Friterie Ch'timi · Les Chineurs

## Méthode (skills du profil `prospect-web`)

1. `prospection-locale` — repérer via Overpass/OSM (`[!website][!"contact:website"]`), filtrer les chaînes, scorer (page sociale détectée = meilleur prospect).
2. **Vérification obligatoire** — recherche web **et** contrôle des domaines évidents (`nom-ville.fr`, `.com`, variantes sans apostrophe). Sur 8 candidats vérifiés le 2026-09-29, 3 écartés : site existant non remonté par la recherche, commerce fermé, enseigne franchisée.
3. `identite-visuelle` — kit d'intake envoyé au client puis extraction de la palette (`brand_kit.py`, ffmpeg) ; jamais de teinte inventée.
4. `dossier-prospect` — trois notes par dossier + mise à jour des index.
5. `prompt-vitrine` — prompt complet (Claude Code / Cursor) entre les marqueurs `PROMPT — DÉBUT/FIN`.
6. Synchroniser (`/opt/data/bin/vault-sync.sh`) et vérifier `HEAD` local = distant.

## Ce qui reste à vérifier

- Villes suivantes à ouvrir (non décidées).
- Chiffres d'activité à confirmer avec Moh Amed : offre exacte, tarif annoncé, sous-domaine, durée de conservation.
- Aucun email public trouvé pour les 8 prospects : le premier contact sera téléphonique ou sur place.

## Sources

- /opt/data/obsidian-vault/AGENTS.md — conventions du vault, consultées le 2026-09-29.
- https://overpass-api.de/api/interpreter — requêtes du 2026-09-29 (Houplines, Armentières, Lille).
- /opt/data/cache/scratch/prospect-demo/ — données brutes (OSM) et vérifications du 2026-09-29.

## Règle de tenue

- Cet index se met à jour à chaque ouverture de ville et à chaque changement de statut, dans la même passe que l'index de ville.
- Les compteurs sont la somme des index de ville : recompter en cas d'écart.
- Ne jamais créer de dossier de premier niveau dans le vault.
