# Vault Obsidian partagé avec Hermes

## Vue d'ensemble

Ce dépôt **est** un vault Obsidian : uniquement des fichiers markdown, aucun code, aucun binaire. Il est partagé entre un humain (Moh Amed, sur Windows via le plugin Obsidian Git) et un agent (Hermes, en conteneur sur un VPS). `main` est la branche partagée : tout ce qui est poussé est visible des deux côtés, et la synchronisation automatique tourne toutes les 15 minutes.

Écrire dans ce dépôt a une conséquence : **une note mal placée ou mal nommée casse les liens de l'autre côté.**

## Commandes

```bash
# Synchroniser (pull --rebase, commit, push ; silencieux si tout va bien)
/opt/data/bin/vault-sync.sh
/opt/data/bin/vault-sync.sh --status      # état courant, sans rien modifier
/opt/data/bin/vault-sync.sh --verbose     # tout ce qui se passe

# Vérifier ce qui est réellement publié
git log --oneline -5
git status --short
git ls-remote origin refs/heads/main      # doit correspondre à `git rev-parse HEAD`
```

Le chemin du vault est donné par `OBSIDIAN_VAULT_PATH` (`/opt/data/obsidian-vault` en conteneur, `/docker/hermes-agent-wezi/data/obsidian-vault` côté VPS).

## Architecture

- `00-Inbox/` — captures brutes, non classées
- `10-Idees/` — idées de contenu, angles, accroches
- `20-Scripts/` — scripts et brouillons de vidéos et de posts
- `30-Sources/` — documentation de référence et sources citées (dont la doc de l'API Hostinger)
- `40-Business/` — projets et études de marché, avec leur suivi
- `50-Connaissance/` — connaissance réutilisable, rangée par domaine (veille technologique)
- `80-Hermes/` — espace propre à l'agent : index, carte de l'environnement, conventions, journal de bord
- `90-Templates/` — modèles de notes
- `90-Templates/instance-typescript/` — **modèles à copier dans un dépôt de code** (`AGENTS.md`, `CLAUDE.md`), qui ne s'appliquent pas à ce vault : un `AGENTS.md` rangé ici ne décrit pas ce dépôt
- `README - Vault partagé avec Hermes.md` — la porte d'entrée du vault

**Attention aux fichiers `AGENTS.md` imbriqués** : leur contenu est chargé comme contexte pour tout agent qui travaille dans leur dossier. Ceux de `90-Templates/` sont des modèles destinés à un autre dépôt — en tenir compte, ne pas les confondre avec le présent contrat.

Chaque dossier de premier niveau a sa note d'index (`<Domaine> — Index.md`) : c'est elle qu'on met à jour en même temps que le contenu.

## Conventions

- **Français**, sauf mention contraire.
- Liens internes en **wikilinks** : `[[Titre de la note]]`.
- **Une note = un sujet.** Un contenu qui couvre trois choses devient trois notes reliées entre elles.
- Nommage des notes : `<Sujet> — <Précision>.md`, avec le tiret cadratin comme séparateur dans la famille `— Index`.
- **Caractères interdits dans les noms de fichiers** : `: ? * " < > | / \`. Un seul suffit à casser la synchronisation selon l'appareil.
- Une note qui affirme quelque chose de vérifiable porte **sa date de vérification** en tête et **ses sources** en pied. La connaissance périssable (outils, tarifs, réglementation) est datée ; ce qui est durable ne l'est pas.
- Séparer explicitement le **vérifié** (lu sur une source primaire ce jour-là) du **rapporté** (source secondaire, non confirmée).
- Ne jamais renommer ni déplacer une note écrite par l'humain sans le lui dire : les wikilinks des autres notes pointent vers son titre.
- Un dossier vide porte un `.gitkeep`.

## Tests

Il n'y a pas de suite de tests ici. La vérification d'un changement de vault se fait ainsi :

```bash
# 1. Aucun secret dans ce qui va être publié
grep -rInE '(api[_-]?key|secret|token|password|passwd|mot de passe|BEGIN [A-Z ]*PRIVATE KEY)[[:space:]]*[:=]' --include='*.md' .
# 2. Tous les liens internes pointent vers une note existante
# 3. HEAD local = HEAD distant après push
```

Un hook `pre-commit` dans `.git/hooks/` bloque déjà les motifs de secret, et `vault-sync.sh` fait le même contrôle avant de pousser.

## Frontières

### Toujours
- Vérifier la présence de secrets **avant** de committer (le hook le fait, mais le contrôle est aussi à la charge de l'auteur).
- Mettre à jour la note d'index du dossier touché, et l'index de branche si le contenu est nouveau.
- Vérifier que les wikilinks ajoutés pointent vers des notes existantes.
- Pousser, puis vérifier que `HEAD` local égale `HEAD` distant.
- Annoncer dans le compte rendu : fichiers touchés, ce qui a été vérifié, ce qui reste ouvert.

### Demander avant
- Supprimer un fichier, ou en renommer un.
- Créer un dossier de premier niveau, ou changer l'organisation générale.
- Modifier une note écrite par l'humain (corriger est bien, écraser ne l'est pas).
- Réorganiser les liens d'un index existant.

### Jamais
- **Publier un secret, sous quelque forme que ce soit, même dans un dépôt privé** : clé, jeton, mot de passe, chaîne de connexion, `.env`, certificat, clé privée SSH. La règle est absolue et ne souffre aucune exception — c'est la raison d'être du hook `pre-commit`.
- Recopier la **valeur** d'un secret dans une note « pour mémoire ». On note où il se trouve et comment le régénérer, jamais le secret lui-même.
- Nommer un dossier `vault` : ce nom est réservé par Hermes à son coffre de mots de passe, l'écriture y échoue.
- `git push --force`, réécrire l'historique (`rebase`, `amend` sur des commits déjà poussés), supprimer une branche : l'humain travaille sur le même dépôt depuis Windows, un historique réécrit détruit son travail.
- Écrire des fichiers binaires, des images ou du code dans le vault.
- Inventer une source, une URL ou une citation. Une affirmation non sourcée se marque « à vérifier ».

## Définition de terminé

Un changement du vault est terminé quand : la note ou le fichier existe et suit les conventions de nommage · les index concernés le référencent · aucun lien mort n'a été introduit · aucun secret n'est présent · `HEAD` local égale `HEAD` distant, vérifié par `git ls-remote`.

## Format du compte rendu

Ce qui a été fait · où (chemins) · ce qui a été vérifié et par quel moyen · ce qui reste ouvert ou incertain. Ne jamais affirmer qu'un contenu est publié sans avoir comparé les deux `HEAD`.
