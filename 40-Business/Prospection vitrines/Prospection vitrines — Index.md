---
tags: [prospection, vitrine, index, business]
statut_global: en cours
villes_total: 2
prospects_total: 298
date_creation: 2026-09-29
date_maj: 2026-09-29
---

# Prospection vitrines — Index

Activité : proposer à des commerces locaux **sans site web** un site vitrine monté en test, gratuit et sans engagement, facturé seulement s'ils le gardent. Un dossier par commerce, trois notes par dossier : fiche, mail icebreaker, note du prompt du site.

Cet index est la racine de l'activité. Le dossier `40-Business/` n'a pas d'index (il n'a pas à en créer un sans l'accord de Moh Amed — signalé ici).

## Les villes

| Ville | Prospects repérés | Dossiers prêts | Contactés | Réponses | Gagnés | Dernière action | Index |
|---|---|---|---|---|---|---|---|
| Houplines | 12 | 1 | 0 | 0 | 0 | 2026-09-29 | [[Houplines — Index]] |
| Lille | 286 | 2 | 0 | 0 | 0 | 2026-09-29 | [[Lille — Index]] |

## Compteurs globaux

| Étape | Nombre |
|---|---|
| Villes ouvertes | 2 |
| Commerces repérés (OSM, sans site déclaré) | 298 |
| Dossiers écrits (`dossier_prêt`) | 3 |
| Contactés | 0 |
| Relancés | 0 |
| Réponses reçues | 0 |
| Sites en test en ligne | 0 |
| Gagnés | 0 |
| Perdus | 0 |

Taux de réponse : non calculable (0 contacté).

## Les dossiers

Chaque dossier vit dans `Prospection vitrines/<Ville>/<Nom du commerce>/` et contient exactement trois notes :

- `<Nom du commerce> — Fiche.md`
- `<Nom du commerce> — Mail icebreaker.md`
- `<Nom du commerce> — Prompt du site.md`

## Méthode (implémentée en skills du profil `prospect-web`)

1. `prospection-locale` — repérer les commerces sans site : Overpass/OSM (`[!website][!"contact:website"]`), filtre des chaînes, scoring des signaux (page Facebook/Instagram détectée = meilleur prospect, téléphone, adresse complète, horaires).
2. Vérification obligatoire — recherche web **et** contrôle des domaines évidents (`nom-ville.fr`, `.com`) : le tag OSM peut être périmé et la recherche web ne remonte pas toujours le site d'un artisan. Un candidat démenti est écarté, avec la preuve.
3. `identite-visuelle` — récupérer ce qui est légitimement accessible (page publique, kit d'intake envoyé au client) puis extraire la palette (`brand_kit.py`, ffmpeg) ; jamais de teinte inventée.
4. `dossier-prospect` — écrire les trois notes du dossier et mettre à jour les index.
5. `prompt-vitrine` — générer le prompt complet (Claude Code / Cursor) qui fait construire le site par une équipe d'agents, entre les marqueurs `PROMPT — DÉBUT/FIN` de la note « Prompt du site ».
6. Synchroniser (`/opt/data/bin/vault-sync.sh`) et vérifier que `HEAD` local égale `HEAD` distant.

## Ce qui reste à vérifier

- Les villes envisagées après Lille et Houplines (non ouvertes).
- Les chiffres d'activité à confirmer avec Moh Amed : offre exacte, tarif annoncé, sous-domaine utilisé, durée de conservation des messages du formulaire.

## Sources

- /opt/data/obsidian-vault/AGENTS.md — conventions du vault en vigueur, consultées le 2026-09-29.
- https://overpass-api.de/api/interpreter — requêtes du 2026-09-29 (Houplines, Lille).
- /opt/data/cache/scratch/prospect-demo/{candidats.json, verif.json} — données brutes et vérifications du 2026-09-29.

## Règle de tenue

- Cet index se met à jour à chaque ouverture de ville et à chaque changement de statut, dans la même passe que l'index de ville.
- Les compteurs sont la somme des index de ville : en cas d'écart, recompter plutôt que corriger au jugé.
- Ne jamais créer de dossier de premier niveau dans le vault : l'activité vit sous `40-Business/`.
