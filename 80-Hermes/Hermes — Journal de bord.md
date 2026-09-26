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

### Branche « Connaissance » et premiers cours

- Demande : une branche qui regroupe la veille technologique par domaines, avec pour première note des cours sur le travail d'agents en équipe (Cursor, Claude, GitHub Copilot) et le rôle de manager.
- Structure créée : `50-Connaissance/` avec [[Connaissance — Index]] (les domaines, les conventions d'écriture) et le premier domaine `IA et agents` avec [[IA et agents — Index]].
- Quatre cours écrits : [[Cursor — agents en équipe]], [[Claude Code — agents en équipe]], [[GitHub Copilot — agents en équipe]], [[Le manager d'équipe — orchestrer des agents]].
- **Vérifié sur les documentations officielles** (consultées le 26/09/2026) et non de mémoire : Cursor (`cursor.com/docs/agent/subagents` — emplacements, compatibilité des dossiers `.claude/` et `.codex/`, champs `readonly` et `is_background`, exécution parallèle) et Claude Code (`docs.claude.com/en/docs/claude-code/sub-agents` — priorité des emplacements, `isolation: worktree`, `memory`, hooks de sous-agent, type `fork`, exécution en arrière-plan systématique) ; GitHub (`docs.github.com` — modes de démarrage de l'agent cloud, champ de consignes, API d'assignation GraphQL et son en-tête obligatoire).
- **Séparation tenue** : ce qui est affirmé vient de la documentation officielle, ce qui vient de guides tiers est marqué « rapporté » dans les notes. Chaque cours porte sa date de vérification, parce que ces outils changent tous les mois.
- Convention de la branche : une note par sujet, sources et date en tête et en pied, les pièges écrits noir sur blanc.

### Équipe d'agents complète — les configs

- Demande : ajouter des exemples de configuration pour une équipe entière — un PO, un manager, un dev front, un dev back, un testeur, un relecteur et un responsable sécurité — afin de livrer du code propre et fonctionnel.
- Note de doctrine : [[Équipe complète de livraison — mode d'emploi]] — les sept rôles, ce que chacun **possède** et ce qu'il **refuse** de faire, les artefacts qui leur servent d'interface (`specs/`, `PLAN.md`, `reviews/`), l'ordre d'exécution, et le rituel de vérification du manager avec ses commandes réelles.
- Trois notes de configuration, une par outil : [[Équipe complète — configs Claude Code]] (sept fichiers `.claude/agents/` avec les prompts détaillés), [[Équipe complète — configs Cursor]], [[Équipe complète — configs GitHub Copilot]] (plus les cinq modèles d'issues qui tiennent lieu de briefs).
- **Point structurel vérifié et intégré** : dans Claude Code comme dans Cursor, un sous-agent **ne peut pas** en engendrer d'autres. Le manager ne peut donc pas être un fichier d'agent parmi les autres — il est la session principale (`claude --agent manager`, ou une règle `.cursor/rules/manager.mdc`). Seuls les *agent teams* expérimentaux de Claude Code permettent une coordination entre agents.
- **Économie signalée** : Cursor lit aussi `.claude/agents/` (vérifié sur sa documentation). Une seule base de fichiers sert donc les deux outils ; `readonly` et `is_background` s'ajoutent côté Cursor là où c'est nécessaire.
- **Réserves écrites noir sur blanc** : les noms d'outils du frontmatter Copilot n'ont pas pu être lus sur la page officielle (contenu non chargé) — deux sources tierces convergentes sont citées et la note renvoie à l'éditeur d'agents de GitHub pour confirmation. Les tarifs Copilot, Agent HQ et les workflows agentiques restent marqués « rapporté ».

### Le contrat commun : AGENTS.md

- Demande : écrire le `AGENTS.md` de projet. Le dépôt visé n'étant pas précisé, deux livrables plutôt qu'une question bloquante.
- **Le modèle expliqué** : [[AGENTS.md — le contrat commun aux agents]] — les huit sections et ce que chacune empêche, les frontières à trois niveaux (et pourquoi « Demander avant » est le niveau qui protège réellement), le modèle complet à copier, un exemple rempli, les deux réserves (ce n'est **pas** une norme mais une convention ; trop long, il ne sert à rien), les pièges, et les deux questions qui vérifient en deux minutes qu'un agent le lit bien.
- **L'instance réelle** : `AGENTS.md` à la racine de ce vault. Ce dépôt est un vrai dépôt git dans lequel un agent travaille, donc le fichier y est appliqué et non décrit : commandes de synchronisation, arborescence, conventions de nommage et de datation, frontières (avec l'interdiction absolue de publier un secret, qui est la raison d'être du hook `pre-commit`), définition de terminé et format du compte rendu.
- **Vérifié** : le hook `pre-commit` a laissé passer ces deux fichiers sans alerte de secret — ce qui est le cas de test intéressant, puisqu'ils parlent de secrets, de `.env` et de clés sans jamais en contenir la valeur.

### L'instance complète : AGENTS.md pour un SaaS TypeScript

- Demande : « une version complète et utilisable ». La pile n'étant pas donnée, je ne l'ai pas inventée au hasard : je suis allé regarder ses dépôts publics (`api.github.com/users/xenoxe/repos`) — le plus récent est en **TypeScript** (`mail`), et son VPS fait tourner Traefik et un registre Docker. Pile retenue : TypeScript strict, Next.js, PostgreSQL, Prisma, Vitest, Playwright, pnpm, déploiement Docker derrière Traefik.
- Livrable : `90-Templates/instance-typescript/AGENTS.md` — **aucun blanc à remplir**, prêt à copier à la racine d'un dépôt — et `CLAUDE.md`, le fichier de renvoi pour les outils qui lisent ce nom-là.
- Contenu au-delà du modèle générique, là où un SaaS se casse réellement : multi-tenant (toute requête filtrée par organisation, la faille qui tue un SaaS B2B) · migrations (jamais modifier une migration appliquée, jamais de migration destructive sans accord) · facturation (centimes entiers, idempotence, l'état de l'abonnement fait foi chez le fournisseur) · RGPD (rien de personnel dans les journaux, export et suppression à la demande) · déploiement (Docker derrière Traefik en HTTPS, secrets hors de l'image).
- **L'hypothèse est écrite en tête du fichier et en fin de fichier** : ce qui dépend de la pile se limite à trois blocs (Pile et Commandes, Architecture, Déploiement), le reste ne dépend pas du cadre.
- **Effet de bord observé et traité** : un fichier nommé `AGENTS.md` rangé dans le vault est chargé comme contexte de dossier par tout agent travaillant à cet endroit — Hermes l'a fait immédiatement après l'écriture. Une note d'avertissement en première ligne du modèle, plus une mention explicite dans le `AGENTS.md` du vault, évitent qu'un agent prenne ce modèle pour le contrat du vault.

### L'instance AGENTS.md corrigée pour la vraie pile

- Moh Amed a donné sa pile réelle : **NestJS** pour l'API, **Vue.js ou React** pour l'interface. Mon instance précédente (Next.js, Prisma, Vitest) était une déduction raisonnable mais fausse sur l'essentiel — remplacée plutôt que laissée en place.
- **Décision prise en cours de route, sur sa demande** : sa pile annonçait SQLite en local et PostgreSQL en production ; il a tranché pour **PostgreSQL partout**, la même version majeure en local, dans les tests et en production. Conséquence : la moitié « deux moteurs » du fichier a disparu, et ce qui restait a été transformé en discipline PostgreSQL.
- **L'analyse du cas écarté est conservée comme justification de la règle** : quand la base locale diffère de la production, une classe entière de défauts reste invisible jusqu'au premier déploiement — types de colonnes acceptés par l'un et refusés par l'autre (`datetime` contre `timestamp`), JSON stocké de deux façons (`jsonb` refusé par SQLite), type d'union TypeScript que la réflexion rend en `Object` et qui **empêche l'application de démarrer**. C'est parce que ces trois pannes ne se déclenchent qu'au déploiement qu'un moteur unique est la règle, et non un confort.
- **Nuance utile gardée en note** : Prisma supporte les énumérations et le JSON sur SQLite **depuis la version 6.2.0**, mais sans aucun contrôle par la base (une valeur invalide écrite hors ORM casse à la lecture, pas à l'écriture) ; les tableaux et les types natifs (`@db.Uuid`) restent réservés à PostgreSQL. La généralité « SQLite ne gère pas les énumérations » qui circule partout est périmée.
- Le dossier a été **renommé** (`instance-typescript` → `instance-nestjs-fullstack`, par `git mv`, pour que l'historique suive) et les quatre références à l'ancien nom ont été mises à jour : `AGENTS.md` du vault, `README`, note du modèle, et ce journal.
- Le contenu de la section base de données, désormais propre à PostgreSQL : version épinglée et identique partout · types de colonnes explicites, aucun type emprunté à un autre moteur · `jsonb` légitime mais conversion `text → jsonb` écrite à la main (le diff automatique ne génère pas la clause `USING`) · préférer un `varchar` validé par DTO à un type enum de base, plus simple à faire évoluer · index sur les colonnes filtrées et triées · pagination par curseur plutôt qu'`OFFSET` · pool de connexions dimensionné · sécurité au niveau des lignes (RLS) comme seconde barrière sur les tables d'organisation · attention aux verrous d'un `ALTER TABLE` qui réécrit une grande table.
- Ajouts venus de la documentation NestJS : les DTO sont des **classes** (les décorateurs de validation ne s'appliquent pas à une interface) · `ValidationPipe` global avec `whitelist` et `forbidNonWhitelisted` · autorisation dans les **gardes**, jamais dans le corps du contrôleur · aucune base atteinte depuis un contrôleur · codes d'erreur Postgres (`23505`, `23503`) traduits en exceptions Nest.

### L'arborescence complète, copiable en l'état

- Demande : « créer chaque répertoire dans le repo, par exemple `.cursor/`, pour copier-coller le répertoire en l'état — n'oublie pas d'y inclure les *rules* ».
- **34 fichiers** générés par un script (`/opt/data/cache/scratch/build-team.py`) plutôt qu'écrits un par un, pour une raison précise : les corps de prompt des six agents sont ainsi **issus d'une source unique** et ne peuvent pas diverger entre Claude Code, Cursor et Copilot. Trois arborescences écrites à la main auraient divergé au premier correctif.
- `.claude/agents/` (7 fichiers, frontmatter complet) · `.cursor/agents/` (6 fichiers, sans le manager — expliqué plus bas) + `.cursor/rules/` (**6 règles** : doctrine du manager, frontières, API NestJS, base de données, interface, tests) · `.github/agents/` (7 agents Copilot), `.github/copilot-instructions.md`, `.github/ISSUE_TEMPLATE/` (5 modèles d'issues suivant le cadre WRAP) · `specs/` et `reviews/`.
- **Le manager n'est pas traité comme les autres, et c'est le point de conception** : un sous-agent ne pouvant pas en engendrer d'autres, il est la **session** sous Claude Code (`claude --agent manager`), une **règle** sous Cursor (`00-manager.mdc`, appliquée à chaque session), et un agent qui produit des ordres de travail sous Copilot — sans pouvoir déclencher les autres, d'où les modèles d'issues. Il n'y a donc volontairement pas de `manager.md` dans `.cursor/agents/`.
- Ajout d'un point pratique qui aurait fait échouer la copie : **Obsidian masque les dossiers commençant par un point**. Les fichiers sont bien sur le disque et synchronisés par Git, mais invisibles dans l'interface — la notice de copie renvoie à l'Explorateur Windows.
- Vérifié : les 34 fichiers sont suivis par git (aucune règle du `.gitignore` ne vise les dossiers en point) et le hook anti-secret est passé.

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
