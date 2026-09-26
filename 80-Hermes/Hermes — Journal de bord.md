---
tags: [hermes, journal, meta]
---

# Hermes — Journal de bord

Ce que j'ai fait dans ce vault, dans l'ordre, avec la preuve. Retour : [[Hermes — Index]]

## 2026-09-26

### Mise en place du vault et de sa synchronisation

- Vault créé dans le volume persistant : `/opt/data/obsidian-vault` (côté hôte : `/docker/hermes-agent-wezi/data/obsidian-vault`), arborescence `00-Inbox`, `10-Idees`, `20-Scripts`, `30-Sources`, `90-Templates`.
- `OBSIDIAN_VAULT_PATH=/opt/data/obsidian-vault` ajouté à `/opt/data/.env` — au passage : un dossier nommé `vault` est refusé en écriture par Hermes (coffre de mots de passe), d'où `obsidian-vault`.
- Dépôt privé `xenoxe/hermes-obsidian` créé, écriture via une clé de déploiement générée ici (`/opt/data/.ssh/id_ed25519_obsidian`) — la clé privée n'est jamais sortie du conteneur.
- Script `vault-sync.sh` + tâche planifiée toutes les 15 min.
- **Vérifié** : push réel relu depuis GitHub (`git ls-remote origin` = HEAD local), pull d'une note écrite depuis Windows réussi.

### Garde-fou anti-secret

- Règle posée (aucun secret sur GitHub, même en dépôt privé) et appliquée techniquement : scan avant commit/push dans `vault-sync.sh`, plus un hook `pre-commit`.
- **Vérifié** par 8 cas de test : secret dans une note → bloqué ; fichier `*.pem` → bloqué ; `.env` → bloqué ; commit manuel avec secret → bloqué par le hook ; titre de note contenant le mot « secret » → laissé passer (faux positif corrigé) ; note propre → poussée.
- Le message d'alerte cite le fichier et le motif, jamais la valeur du secret.

### Documentation de l'API Hostinger

- 8 notes dans `30-Sources/Hostinger API/`, générées depuis le **spec OpenAPI officiel v1.54.2** (392 endpoints, 11 produits) — pas de mémoire. Entrée : [[Hostinger API — Index]].
- **Vérifié** : les 8 fichiers présents sur le distant, garde-fou passé sur ~294 Ko de contenu, aucun jeton en clair (les exemples utilisent `$HOSTINGER_API_KEY`).

### Ouverture de cet espace personnel

- Dossier `80-Hermes/` créé à la demande de Moh Amed, avec cet index, [[Hermes — Carte de l'environnement]], [[Hermes — Fonctionnement du vault]], [[Hermes — Conventions et garde-fous]] et ce journal.

### Étude de marché SaaS

- Demande : identifier un créneau SaaS à revenu récurrent, tenable en position de leader par un fondateur solo technique, pour un lancement France/francophone.
- Méthode : 5 axes de recherche parallèles (conformité récurrente TPE · facturation électronique 2027 · verticaux mal servis · IA verticale artisanat · francophonie hors France), ~130 sources, puis **vérification personnelle des 13 faits qui décident du choix** — un rapport d'agent est une déclaration, pas une preuve.
- 4 notes dans `40-Business/Étude SaaS 2026/`, entrée : [[Étude SaaS — Index]]. Verdict : **l'échéancier de conformité opposable pour l'artisanat du bâtiment** (32/40), avec plan B pompes funèbres et plan C auto-écoles.
- **Vérifié moi-même** : sanction DUERP jusqu'à 4 000 €/manquement (loi du 25 juin 2026, service-public A18908) · Klaxo 99/159/249 € HT/mois · arrêté auto-écoles du 9 février 2026 · CPF permis plafonné à 900 € depuis le 20 février 2026 · IArtisans CAPEB à 29,95 €/mois · Costructor gratuit + 12,50/25/50 € · Agendrop 19/29 €.
- **Ce que cette étude n'est pas** : une validation. Elle débouche sur un protocole d'essai à 14 jours, sans développement, avec 4 critères d'arrêt explicites.

### Veille hebdomadaire et premier correctif

- Demande : un rapport **résumé sur Telegram** chaque semaine + un rapport **détaillé dans le vault**. Job `veille-conformite-artisans` (`1717de3e8043`) créé, **lundi 7h UTC** (9h Paris), continuité activée pour qu'il signale les changements et non l'état, livraison dans la conversation Telegram (répondable).
- Structure posée : `Veille/Veille — Index.md` (ce qui est surveillé, règle d'écriture) + une note datée par semaine + `Suivi de validation.md`, que le job relit à chaque passage pour reprendre l'avancement réel de Moh Amed.
- Premier run déclenché à la main pour vérifier : **il a trouvé, en 2 minutes, un fait qui contredisait un pilier de l'étude** (Cloud VGP, offre unifiée à 0,50 €/équipement/mois, DUERP et archivage horodaté inclus). Note de 19 Ko écrite et poussée.
- J'ai revérifié le fait moi-même puis **corrigé l'étude** : la phrase « personne ne vend l'ensemble au prix d'une TPE » est retirée, le critère « faiblesse de la concurrence » passe de 3 à 2, le créneau de **32 à 31/40**, un J0 de qualification d'une heure et un cinquième critère d'arrêt sont ajoutés au protocole.
- **Leçon retenue dans le skill `market-opportunity-scan`** (v1.1.0) : une veille qui ne corrige jamais l'étude n'est pas une veille. Le job a servi dès son premier run — c'est l'argument pour installer cette boucle systématiquement, pas seulement quand on y pense.

## Points ouverts

- **Obsidian côté Windows** : l'installation du plugin Obsidian Git et le clone sont à confirmer côté PC (voir [[Hermes — Fonctionnement du vault]]).
- **Vault préexistant ?** La question n'a pas eu de réponse : s'il existait déjà un vault avec des notes sur son PC, il faudrait fusionner plutôt que de garder deux historiques parallèles.
- **Historique du dépôt** : proposé — vérifier qu'aucun secret n'a été poussé avant la mise en place du garde-fou (le dépôt est né aujourd'hui, donc a priori non, mais à confirmer).

## Comment vérifier ce journal

```bash
cd /opt/data/obsidian-vault
git log --oneline                       # l'historique réel, avec les dates
git ls-remote origin                    # ce que GitHub a effectivement reçu
/opt/data/bin/vault-sync.sh --status    # état courant
```
