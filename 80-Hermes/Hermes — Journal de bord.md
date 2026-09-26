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
