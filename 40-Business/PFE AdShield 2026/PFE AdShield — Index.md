---
tags: [pfe, projet, reseau, dns, publicite]
---

# PFE AdShield — Index

Projet de fin d'études : **programme qui bloque la publicité au niveau du réseau**, utilisable par tous les appareils du foyer, plus une extension navigateur pour ce que le DNS ne peut pas faire.

- Contrainte annoncée : **rendu en moins d'un mois**.
- Livrable choisi par Moh Amed : **bloqueur DNS/hosts + extension** (option la plus simple des quatre proposées).
- Services visés : **Twitch** et **YouTube** (+ sites web en général).
- Code : `/opt/data/projects/adshield` (Node 26 + TypeScript, zéro dépendance d'exécution).

## État au 2026-09-27

| lot | état |
|---|---|
| Moteur DNS (UDP+TCP, cache, relais, stats) | fait et vérifié |
| Filtrage listes publiques + exceptions | fait et vérifié — 127 846 domaines |
| API HTTP + tableau de bord | fait et vérifié |
| Tests (unitaires + bout-en-bout) | fait — 16/16 |
| Extension navigateur (MV3) | à faire |
| Module Twitch HLS (hors DNS) | à décider — voir [[PFE AdShield — Architecture et limites]] |

## Décisions à prendre

1. **Extension navigateur** : périmètre exact (filtres réseau type EasyList + habillage cosmétique) et navigateur cible (Firefox plus permissif que Chrome en MV3).
2. **Partie Twitch « in-stream »** : le DNS ne peut pas la traiter. Deux options pour le mémoire : l'assumer comme limite démontrée (le plus honnête en 4 semaines) ou ajouter un module HLS qui supprime les segments pub (jouable mais fragile, et contraire aux CGU de Twitch).
3. **Déploiement de démonstration** : sur le VPS Hostinger (`srv1398132.hstgr.cloud`) avec exposition HTTPS Traefik, ou simplement sur un PC du réseau local ? La démo « tous les appareils filtrés » se fait sur le réseau local.

## Points d'entrée

- [[PFE AdShield — Architecture et limites]] — technique : pourquoi le DNS suffit pour la pub classique et pas pour Twitch, plan de mesure, chiffres relevés.
- Code : `/opt/data/projects/adshield` (`README.md` en porte d'entrée).
- Notes voisines : [[Hermes — Carte de l'environnement]], [[Étude SaaS — Index]]
