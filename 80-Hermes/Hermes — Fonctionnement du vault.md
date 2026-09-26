---
tags: [hermes, obsidian, sync, meta]
---

# Hermes — Fonctionnement du vault

Comment ce vault est synchronisé entre le serveur (moi) et le PC Windows (lui). Retour : [[Hermes — Index]]

## La chaîne

```
Obsidian (Windows)  ⇄  GitHub  ⇄  /opt/data/obsidian-vault (conteneur)
   plugin Obsidian Git         dépôt privé xenoxe/hermes-obsidian
```

| Maillon | Détail |
|---|---|
| Dépôt | `git@github.com:xenoxe/hermes-obsidian.git` (privé), branche `main` |
| Auth serveur | clé de déploiement SSH `/opt/data/.ssh/id_ed25519_obsidian` (droits d'écriture, liée au dépôt uniquement) |
| Côté Windows | plugin **Obsidian Git** : auto-pull ~10 min, commit-and-sync ~10 min |
| Côté serveur | script `vault-sync.sh` + tâche planifiée `obsidian-vault-sync` toutes les 15 min |

Le **coffre de mots de passe** de Hermes porte aussi le nom `vault` : un dossier nommé `vault` est protégé en écriture par Hermes, d'où le nom `obsidian-vault`.

## Le script

```bash
/opt/data/bin/vault-sync.sh            # sync silencieuse (mode tâche planifiée)
/opt/data/bin/vault-sync.sh --verbose  # affiche aussi les succès
/opt/data/bin/vault-sync.sh --status   # état du dépôt, sans rien modifier
```

Ce qu'il fait, dans l'ordre :

1. `git pull --rebase --autostash` — récupère ce qui a été écrit depuis Obsidian. Le `--autostash` fait qu'aucune note locale non committée n'est perdue, et le `--rebase` garde un historique linéaire (pas de merge bruyant).
2. **Scanne** ce qui serait committé à la recherche de secrets → s'il en trouve, il s'arrête net (voir [[Hermes — Conventions et garde-fous]]).
3. `git add -A` + commit daté, seulement s'il y a du changement.
4. `git push` uniquement si un commit local attend, ou si `origin/main` est en retard.

Sortie vide = tout va bien (c'est le mode veille de la tâche planifiée : `no_agent`, sortie vide = aucun message envoyé, sortie non vide = alerte Telegram).

## Dépannage

| Symptôme | Cause probable | Quoi faire |
|---|---|---|
| `pull en échec` dans le message d'alerte | conflit : la même note a été modifiée des deux côtés | ouvrir la note, garder les deux contenus, recommitter à la main |
| `push en échec` | clé retirée du dépôt, ou dépôt renommé | vérifier la clé de déploiement dans les réglages du dépôt |
| Commit manuel refusé par un « 🚫 COMMIT BLOQUÉ » | le hook pre-commit a trouvé un motif de secret | retirer le secret de la note (ou `git rm --cached <fichier>`) |
| Toutes les notes arrivent d'un coup dans Obsidian | normal : le vault vit côté serveur, Obsidian le rattrape au pull suivant | rien |
| Un dossier vide disparaît | Git ne versionne pas les dossiers vides | c'est pour ça que chaque dossier contient un `.gitkeep` |

## Réinstaller les protections après un nouveau clone

Les hooks Git ne sont **pas** versionnés : après un clone neuf, il faut remettre le garde-fou.

```bash
cp /opt/data/scripts/pre-commit-secret-guard.sh /opt/data/obsidian-vault/.git/hooks/pre-commit
chmod +x /opt/data/obsidian-vault/.git/hooks/pre-commit
cp /opt/data/scripts/obsidian-vault-sync.sh /opt/data/scripts/   # déjà en place ici
```
