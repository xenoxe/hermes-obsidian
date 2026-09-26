---
tags: [hermes, securite, conventions, meta]
---

# Hermes — Conventions et garde-fous

Mes règles d'écriture dans ce vault, et mes interdits. Retour : [[Hermes — Index]]

## Règle absolue : aucun secret dans le vault

**Aucune clé, aucun jeton, aucun mot de passe, aucun `.env` — jamais, même dans un dépôt privé, même « juste pour tester ».** Un dépôt privé est un dépôt qui finira public un jour, ou qui sera partagé ; et l'historique Git garde tout, même après suppression du fichier.

Deux barrières partagent la même liste de motifs (`/opt/data/bin/secret-patterns.txt`) :

| Barrière | Quand | Effet |
|---|---|---|
| `vault-sync.sh` | avant chaque commit/push | refuse le commit et le push, signale le fichier et le motif — jamais la valeur du secret, pour ne pas la faire fuiter dans Telegram |
| Hook `pre-commit` | à chaque `git commit`, même manuel | bloque le commit sur le contenu indexé |

Ce qui est détecté : clés privées PEM, AWS `AKIA…`, jetons GitHub (`ghp_`, `github_pat_`), OpenAI `sk-…`, Slack `xox…`, Google `AIza…`, JWT (`eyJhbGciOi…`), URL de base de données avec mot de passe, `*API_KEY* / *TOKEN* / *SECRET* / *PASSWORD* = <valeur>`, `Authorization: Bearer <valeur>`, et une liste de noms de fichiers sensibles (`.env`, `*.pem`, `id_rsa*`, `*credentials.json`…).

Comment écrire un exemple d'appel d'API dans une note, alors ? **Toujours la variable, jamais la valeur** :

```bash
curl https://developers.hostinger.com/api/vps/v1/virtual-machines \
  -H "Authorization: Bearer $HOSTINGER_API_KEY"
```

Pour ajouter un motif nouvellement appris, on l'écrit **une seule fois** dans `secret-patterns.txt` (format `grep -E`, une expression par ligne, sans commentaire) : les deux barrières le prennent en compte.

> [!note] Piège appris à l'usage
> Ne jamais mettre de motif large du genre `*secret*` dans la règle sur les **noms de fichiers** : une note légitime intitulée « Les secrets d'une bonne accroche » serait bloquée. Les vrais secrets se détectent par le **contenu**.

## Conventions d'écriture

- **Langue** : français, sauf les termes techniques et le contenu d'origine (API, code, titres officiels).
- **Nom des notes** : `Sujet — Précision` (tiret cadratin), une note = un sujet.
- **En-tête** : petit bloc `tags:` en haut, en minuscules (`hostinger`, `api`, `reference`, `hermes`…).
- **Liens** : wikilinks `[[Nom de la note]]` dès qu'une note en évoque une autre, et un lien de retour vers la note parente en haut.
- **Référence vs explication** : les tableaux denses pour les références (endpoints, codes, paramètres), la prose pour le raisonnement et les pièges.
- **Appels** : `> [!warning]` pour un risque, `> [!tip]` pour une astuce, `> [!note]` pour un contexte. Utilisés avec parcimonie, sinon ils ne signalent plus rien.
- **Faits vérifiables** : une affirmation chiffrée (« 392 endpoints ») dit d'où elle vient et à quelle date.

## Ce que je ne fais pas

- **Ne pas réécrire les notes de Moh Amed** dans `00-Inbox`, `10-Idees`, `20-Scripts` : j'y ajoute sur demande, je ne restructure pas.
- **Ne pas pousser du contenu non vérifié** : préférer « je n'ai pas vérifié » à une affirmation plausible.
- **Ne pas toucher** au dépôt d'un autre profil Hermes, ni à `srv788682.hstgr.cloud` (voir [[Hermes — Carte de l'environnement]]).
- **Ne jamais nettoyer un secret après coup** : si un secret a été poussé une fois, la seule sortie propre est de le **révoquer** — l'historique garde la trace.

## Avant de conclure une tâche

1. `/opt/data/bin/vault-sync.sh` — rien ne sert d'écrire une note qui reste sur le serveur.
2. Vérifier que le distant a bien reçu : `git ls-remote origin` doit renvoyer le même commit que `git log --oneline -1`.
