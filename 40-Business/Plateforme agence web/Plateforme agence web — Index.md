---
tags: [infrastructure, agence, hostinger, docker, traefik, signoz]
verifie_le: 2026-09-29
---

# Plateforme agence web — Index

Plateforme **100 % auto-hébergée** pour héberger et maintenir les sites et API de plusieurs clients, sur le VPS Hostinger de Moh Amed. Entrée du dossier.

## Les notes

- [[Infrastructure — Audit du 2026-09-29]] — l'état réel du serveur : ce qui tourne déjà, ce qui est exposé, ce qui manque, et ce qui n'a pas pu être vérifié
- [[Infrastructure — Plan cible et décisions]] — l'architecture cible, les installations, les ports, les réseaux, les volumes, les ressources, les conflits et les **décisions à trancher**

## Le cadrage de la demande

| Contrainte | Ce qui a été retenu |
|---|---|
| Gratuité | aucune licence, aucun abonnement obligatoire, aucun SaaS payant requis |
| Observabilité | **SigNoz seul** — ni Grafana, ni Prometheus, ni Loki, ni Tempo séparés |
| Reverse proxy | **Traefik** (déjà en place) |
| Sécurité | HTTPS partout, aucune base exposée, pare-feu, secrets hors des fichiers versionnés |
| Échelle | passer de 3 à 50 sites sans reconstruire l'infrastructure |
| Isolation | un client ne voit ni les ressources ni les statistiques d'un autre |

## L'état en trois lignes

1. **Le serveur est déjà bien équipé** : Traefik, SigNoz, Zitadel, Hermes et Docker tournent, et **HTTPS avec Let's Encrypt fonctionne déjà** sur la convention `<projet>.srv1398132.hstgr.cloud`.
2. **Deux vrais défauts de sécurité** : l'interface d'administration de SigNoz est **publique en HTTP clair sans authentification**, et il n'y a **aucun pare-feu**.
3. **Un seul obstacle de fond** : la VM est en **2 vCPU / 8 Go**, déjà à 2,2 Go de moyenne avec un pic observé à **7,7 Go**. L'empilement complet (Uptime Kuma, Dozzle, Backrest, Umami, collecteur d'hôte) demande de trancher entre **passer en 16 Go** ou **plafonner SigNoz sévèrement**.

## Le point qui bloque tout

**Rien ne sera modifié avant validation.** Les sept décisions à prendre sont listées au §9 de [[Infrastructure — Plan cible et décisions]] — la plus urgente étant de savoir **qui a lancé les onze opérations Docker du 29 septembre 2026** sur cette machine.
