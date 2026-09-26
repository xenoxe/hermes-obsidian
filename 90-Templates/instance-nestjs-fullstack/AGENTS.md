> **Modèle à copier dans un dépôt de code.** Ce fichier ne s'applique pas au vault où il est rangé : il est destiné à être copié à la racine d'un projet NestJS (+ interface Vue ou React) sous le nom `AGENTS.md`.

# <Nom du SaaS>

Application SaaS B2B pour le marché français (TPE, artisans, PME), en abonnement à revenu récurrent.

**Pile** :
Node 22 · TypeScript strict · **NestJS** (API) · **Vue 3 ou React** + Vite (interface) · **PostgreSQL partout** — local, tests, production · Jest + Supertest (API) · Vitest (interface) · Playwright (bout en bout) · pnpm (espaces de travail) · ESLint + Prettier · Docker derrière Traefik.

**Organisation** : `api/` (NestJS) · `web/` (interface) · `shared/` (types partagés entre les deux) · `docker-compose.yml` (PostgreSQL local).

## Commandes

```bash
pnpm install                              # installer tout (espaces de travail)

# --- api/ ---
pnpm --filter api start:dev               # serveur de développement (PostgreSQL local requis)
pnpm --filter api build                   # construction
pnpm --filter api test                    # Jest : unitaires + intégration
pnpm --filter api test -- src/users/users.service.spec.ts   # UN seul fichier
pnpm --filter api test -- -t "rejette"    # un seul test par son nom
pnpm --filter api test:e2e                # Supertest sur l'application complète
pnpm --filter api lint                    # (lint:fix pour corriger)
pnpm --filter api typecheck               # tsc --noEmit

# --- web/ ---
pnpm --filter web dev
pnpm --filter web build
pnpm --filter web test                    # Vitest
pnpm --filter web test -- src/components/Bouton.spec.ts     # UN seul fichier
pnpm --filter web test:e2e                # Playwright
pnpm --filter web lint
pnpm --filter web typecheck

# --- PostgreSQL local (même version majeure que la production) ---
docker compose up -d db                   # base de développement
docker compose up -d db-test              # base de test (isolée)
docker compose exec db psql -U postgres -d app   # console SQL

# --- migrations (Prisma) ---
pnpm --filter api prisma migrate dev --name <nom>
pnpm --filter api prisma migrate deploy   # en production
pnpm --filter api prisma generate
pnpm --filter api prisma studio

# --- migrations (TypeORM, si c'est l'ORM retenu) ---
pnpm --filter api migration:generate src/database/migrations/<Nom> -d src/database/datasource.ts
pnpm --filter api migration:run
pnpm --filter api migration:revert
```

**Avant de soumettre : `test`, `typecheck` et `lint` passent — lancés par celui qui l'affirme, jamais rapportés de mémoire.**

## Architecture

### API (`api/src/`)

- Un **module par domaine métier** (`users/`, `factures/`, `auth/`). Pas de module fourre-tout.
- Dans chaque module : `*.controller.ts` (routes), `*.service.ts` (logique métier), `dto/` (entrées validées), `entities/` ou `*.repository.ts` (accès aux données), `*.spec.ts` (tests unitaires).
- `common/` — filtres d'exception, gardes, intercepteurs, décorateurs partagés.
- `config/` — configuration typée, chargée par `@nestjs/config`.
- `database/` — `datasource.ts` pour la CLI des migrations, et `migrations/`.
- `test/` — tests de bout en bout (Supertest).

### Interface (`web/src/`)

- `components/` — composants présentationnels, sans accès aux données.
- `views/` (Vue) ou `pages/` (React) — les écrans, qui assemblent.
- `stores/` (Pinia) ou `hooks/` (React) — l'état et les appels d'API.
- `api/` — le client d'API typé, **le seul endroit qui appelle `fetch`**.

### Partagé (`shared/`)

- Types d'entrée et de sortie des routes, enums métier, constantes. **La source unique du contrat d'API** : l'API et l'interface importent d'ici, personne ne redéclare une forme de données de son côté.

## Conventions — API NestJS

- **Les DTO sont des classes, jamais des interfaces.** Les décorateurs de `class-validator` ne s'appliquent qu'à des instances de classe. Une interface ne valide rien.
- **Chaque propriété d'un DTO porte au moins un décorateur de validation.** Un champ sans décorateur est un champ non validé.
- Les champs facultatifs utilisent `@IsOptional()`, jamais le seul `?` de TypeScript (qui n'existe plus à l'exécution).
- Les objets imbriqués passent par `@ValidateNested()` + `@Type(() => SousDto)`.
- `ValidationPipe` global avec `whitelist: true`, `forbidNonWhitelisted: true` et `transform: true` : toute propriété non déclarée est rejetée, pas ignorée silencieusement.
- **Les contrôleurs restent minces** : ils reçoivent, délèguent au service, renvoient. **Aucun accès à la base depuis un contrôleur.**
- L'autorisation vit dans les **gardes**, jamais dans le corps du contrôleur.
- Une **exception Nest** est levée pour tout échec attendu (`NotFoundException`, `ConflictException`, `ForbiddenException`…), jamais une erreur brute.
- Les erreurs de base sont traduites : `23505` (violation d'unicité) → `ConflictException` ; `23503` (clé étrangère) → `BadRequestException`. L'utilisateur ne voit jamais un message de PostgreSQL.
- Une **forme d'erreur unique** pour toute l'API, produite par un filtre d'exception global : code, message, détail éventuel. Jamais de trace de pile dans la réponse.
- Pas de `forwardRef()` : une dépendance circulaire se règle en restructurant les modules.
- Aucun identifiant d'ORM ou de base ne fuit dans l'interface : l'API expose ses propres types, ceux de `shared/`.

## Conventions — interface

- **Un seul client d'API typé** dans `web/src/api/`, qui consomme les types de `shared/`. Aucun `fetch` dispersé dans les composants.
- Aucune logique métier dans un composant : il affiche et il remonte des événements.
- État partagé dans les magasins (Pinia) ou des crochets (React) — pas dans les composants.
- **Identifiants en anglais, textes affichés en français** ; aucun texte en dur dans un composant, tout passe par le fichier de traduction.
- Montants reçus en centimes, affichés en euros. **Aucun calcul monétaire côté interface** : les totaux viennent de l'API.
- Dates reçues en ISO 8601 UTC, affichées en `jj/mm/aaaa`.
- Nommage : `ComposantEnPascalCase.vue|tsx`, `utilitaireEnCamelCase.ts`, `dossier-en-kebab-case/`.

## Base de données et migrations — PostgreSQL partout

**Un seul moteur, la même version majeure en local, dans les tests et en production.**

C'est une décision, pas un détail : quand la base locale diffère de la production, une classe entière de défauts reste invisible jusqu'au premier déploiement — types de colonnes acceptés par l'un et refusés par l'autre, JSON stocké de deux façons, contraintes absentes en local, requêtes valides sur un moteur et invalides sur l'autre. Avec un moteur unique, **ce qui passe en local est ce qui tournera en production**.

### Règles

1. **La version de PostgreSQL est épinglée** (`postgres:16`, jamais `latest`), et c'est **la même image** en local, dans la CI et en production. Une version majeure d'écart, c'est un plan d'exécution, un comportement de tri ou un support de syntaxe qui changent.
2. **La base locale tourne réellement** (`docker compose up -d db`) : l'API ne démarre pas sans elle. Pas de repli silencieux vers un autre moteur, pas de mode dégradé.
3. **Types de colonnes explicites.** Toute colonne dont le type TypeScript est ambigu — union, `null` inclus — porte un type de colonne déclaré. Sans cela, la réflexion TypeScript rend un objet générique, l'ORM ne sait pas choisir de type, et l'application refuse de démarrer.
4. **Aucun type de colonne emprunté à un autre moteur** (`datetime` au lieu de `timestamp`, par exemple) : ce qui n'existe pas dans PostgreSQL ne se déclare pas.
5. **JSON** : `jsonb` est légitime ici, mais une conversion `text → jsonb` exige une clause `USING` que les diffs automatiques ne génèrent pas. Une bascule vers `jsonb` s'écrit donc dans une migration rédigée à la main, pas laissée à l'outil.
6. **Énumérations** : un type enum de base se modifie par `ALTER TYPE`, ce qui est une migration à part. Pour la plupart des besoins, un `varchar` validé par DTO et une contrainte `CHECK` sont plus simples à faire évoluer.
7. **Index** : toute colonne filtrée ou triée de façon courante porte un index. Les clés étrangères en portent un. Un index inutile se supprime.
8. **Pagination par curseur** (`WHERE id > <dernier vu>`) sur les tables qui grossissent, jamais `OFFSET` en profondeur : `OFFSET` lit et jette toutes les lignes précédentes.
9. **Pool de connexions** dimensionné explicitement en production (maximum, minimum, délai d'inactivité), pas laissé aux valeurs par défaut. Sur un VPS unique, la somme des pools de toutes les instances doit rester sous la limite de connexions du serveur.
10. **Sécurité au niveau des lignes** : PostgreSQL sait filtrer les lignes par rôle (RLS). À considérer comme seconde barrière sur les tables d'organisation — elle ne remplace pas le filtre applicatif, elle le rattrape.

### Migrations

- **`synchronize` (TypeORM) reste désactivé partout sauf sur une base locale jetable.** En production, il produit des `ALTER TABLE` inférés qui suppriment des colonnes et des données sans avertissement.
- **Une migration se génère contre PostgreSQL, et se relit à la main avant d'être validée.** Les diffs automatiques sont bruités — en particulier sur les énumérations, les valeurs par défaut et les renommages (un renommage de colonne ressort souvent en suppression puis ajout).
- **Une migration déjà appliquée ne se modifie jamais.** L'outil la suit par nom de fichier : la retoucher ne la rejouera pas et le schéma réel divergera de ce que dit le fichier. Une correction est une nouvelle migration.
- **Toute migration a un `down()`**, et il est testé avant d'être considéré comme écrit. Le jour où on en a besoin est le pire jour pour le découvrir.
- Un changement cassant suit le schéma **ajouter → remplir → basculer → supprimer**, en plusieurs déploiements, chacun compatible avec les deux versions du code en circulation.
- **Les migrations tournent en une étape unique du déploiement**, avant que la nouvelle version ne reçoive du trafic — jamais comme effet de bord du démarrage de chaque instance, sinon plusieurs conteneurs tentent le même `ALTER TABLE` en même temps.
- **Attention aux verrous** : un `ALTER TABLE` qui ajoute une colonne `NOT NULL` avec valeur par défaut peut réécrire toute la table et bloquer les lectures et écritures pendant l'opération. Sur une table volumineuse, on procède en étapes (colonne nullable, remplissage par lots, contrainte ensuite).
- Une migration qui se termine bien n'est pas une migration réussie : elle est réussie quand la nouvelle version du code fonctionne contre le schéma obtenu.

## Multi-tenant et sécurité

- **Toute requête touchant des données d'organisation est filtrée par l'identifiant d'organisation.** Une requête non filtrée est un défaut critique, pas un oubli mineur — c'est la faille qui tue un SaaS B2B.
- L'authentification **et** l'autorisation sont vérifiées par des gardes sur chaque route ; une autorisation manquante est critique.
- Les secrets viennent de `@nestjs/config` et de l'environnement. On les nomme par leur variable (`DATABASE_URL`, `JWT_SECRET`), jamais par leur valeur. Aucun secret dans le dépôt, un test, un commentaire ou un fichier d'exemple.
- Un webhook vérifie sa signature avant tout traitement et reste idempotent.
- Aucune donnée personnelle dans les journaux. Les messages d'erreur ne révèlent pas l'intérieur du système.
- Mots de passe hachés (argon2 ou bcrypt), jamais stockés ni journalisés. Jeton d'accès à durée courte, jeton de rafraîchissement révocable.
- Données personnelles : on ne collecte que le nécessaire, et ce qui est collecté doit pouvoir être exporté et supprimé à la demande (RGPD).
- **Sauvegardes** : une sauvegarde dont on n'a jamais testé la restauration n'est pas une sauvegarde.
- Toute dépendance ajoutée doit être connue, maintenue et nécessaire.

## Tests

- **API : Jest** (`@nestjs/testing`, `Test.createTestingModule`) pour l'unitaire et l'intégration, **Supertest** pour les contrôleurs. Ne pas simuler le module entier : ne simuler que les dépendances directes.
- **Interface : Vitest** (+ Vue Test Utils ou React Testing Library).
- **Bout en bout : Playwright**, sur l'application complète.
- **Un test par comportement**, nommé par le comportement attendu (`rejette une facture sans client`), jamais par le nom de la fonction.
- **Les tests d'intégration s'exécutent contre la base PostgreSQL de test** (`db-test`), jamais contre la base de développement : chaque exécution doit pouvoir repartir d'un état connu. Les deux approches qui fonctionnent : chaque test dans une transaction annulée à la fin, ou un schéma recréé par fichier de test.
- **Couvrir les chemins d'erreur** : entrée invalide, entrée vide, champ non déclaré rejeté par le pipe, absence d'authentification, accès à une ressource d'une autre organisation, ressource absente, quota dépassé.
- Trois scénarios de bout en bout restent verts en permanence : inscription, connexion, première souscription.
- Les tests existants continuent de passer. Modifier un test mérite une explication dans la PR.
- On n'affaiblit pas un test, on ne le marque pas ignoré, on ne désactive pas une vérification de types pour faire passer la construction.

## Facturation

- Montants en **centimes entiers**, jamais en flottant. Aucun calcul monétaire en nombre à virgule, nulle part. En base : `integer` ou `bigint`, jamais `real` ni `double precision`.
- Toute opération de facturation est **idempotente** (clé d'idempotence), parce qu'un webhook arrive parfois deux fois.
- L'état de l'abonnement fait foi **chez le fournisseur de paiement** ; on ne le déduit pas d'un calcul local ni d'une date stockée en base.
- Toute modification touchant les prix, les quotas, la période d'essai ou les remises se **demande avant**.
- Les événements de facturation sont journalisés sans donnée de carte ni jeton.

## Déploiement

- Conteneur Docker derrière **Traefik en HTTPS** sur le VPS. Pas de port publié à la main, pas de HTTP brut.
- L'image de production ne contient pas les dépendances de développement. Les secrets arrivent par variables d'environnement, jamais dans l'image ni dans le dépôt.
- Une variable d'environnement nouvelle est ajoutée au fichier d'exemple **sans sa valeur** et signalée dans la PR — sinon le déploiement suivant échoue en silence.
- Les migrations sont une étape du déploiement, avant la mise en service de la nouvelle version.
- La version de PostgreSQL en production est la même que celle du `docker-compose.yml` local : c'est la règle, elle se vérifie à l'œil sur les deux fichiers.

## Frontières

### Toujours
- `test`, `typecheck` et `lint` passent, lancés par celui qui l'affirme.
- Lire les fichiers voisins avant d'écrire, et suivre les motifs existants du module.
- Un DTO classe avec décorateurs pour chaque entrée nouvelle, enregistré dans le `ValidationPipe`.
- Filter par organisation toute requête sur une table d'organisation.
- Committer sur sa propre branche, un sujet par branche.
- Nommer dans le rapport final : fichiers touchés, commandes lancées, résultat réel.

### Demander avant
- Modifier le schéma de base de données, ou écrire une migration destructive.
- Ajouter une dépendance, même petite, même « évidente ».
- Modifier le contrat d'API : une route existante, la forme d'une réponse, un type de `shared/`.
- Toucher à l'authentification, à l'autorisation, aux jetons, aux webhooks, à la facturation, aux quotas.
- Modifier la CI, la configuration TypeScript, le `Dockerfile`, la configuration Traefik, les variables d'environnement.
- Découper ou renommer un module existant.
- Ajouter un service externe.
- Changer la version majeure de PostgreSQL.

### Jamais
- Lire, écrire ou recopier un `.env`, une clé, un jeton, un certificat, une chaîne de connexion — pas même dans un test ou un commentaire.
- Mettre `synchronize: true` sur une base qui contient des données réelles.
- Modifier une migration déjà appliquée, ou lancer une commande de base de données pointant vers la production.
- Écrire une requête sur une table d'organisation sans filtre par organisation.
- Accéder à la base depuis un contrôleur ou depuis un composant d'interface.
- Déclarer un DTO en `interface`, ou laisser une propriété sans décorateur de validation.
- Utiliser `any`, désactiver une règle ESLint ou un contrôle de types pour faire passer la construction.
- Déclarer un type de colonne qui n'existe pas dans PostgreSQL.
- Un calcul monétaire en flottant.
- `git push --force`, réécrire l'historique, supprimer une branche, fusionner sans relecture humaine.
- Désactiver, ignorer ou affaiblir un test.

## Définition de terminé

Une tâche est terminée quand :
les tests de l'`api` et du `web` passent (lancés par l'agent qui l'affirme) ·
`typecheck` et `lint` passent sans avertissement nouveau ·
les critères d'acceptation visés sont couverts par un test ·
les chemins d'erreur sont couverts, pas seulement le cas nominal ·
les tests d'intégration ont tourné contre la base PostgreSQL de test ·
les migrations nouvelles ont un `down()` et il a été essayé ·
aucune frontière n'a été franchie ·
aucun secret, aucune donnée personnelle dans les journaux ou le diff ·
les types partagés de `shared/` sont à jour si le contrat d'API a changé ·
la PR nomme les fichiers touchés, les commandes lancées et ce qui reste incomplet.

## Format du rapport final

```
Livré      : <ce qui fonctionne maintenant>
Critères   : <critères couverts, critères non couverts>
Commandes  : <commande> → <résultat réel>
Base       : <tests d'intégration contre PostgreSQL : oui/non>
Incomplet  : <ce qui reste, et pourquoi>
Divergence : <tout écart avec la spécification>
```

Ne jamais affirmer qu'un test passe sans l'avoir lancé. « Devrait fonctionner » n'est pas un résultat.

## Si ta pile diffère

- **ORM** : les règles de la section base de données valent pour Prisma comme pour TypeORM. Si tu utilises Prisma, remplace les commandes `migration:*` de TypeORM par `prisma migrate` ; si tu utilises TypeORM, l'inverse. Le reste ne change pas.
- **Interface** : Vue ou React, seules la section « interface » et les commandes `web` sont concernées. Supprime la variante que tu n'utilises pas (`stores/` ou `hooks/`, Vue Test Utils ou React Testing Library).
- **Express au lieu de NestJS** : la section « API NestJS » tombe en grande partie (DTO, `ValidationPipe`, gardes). Garde la validation à la frontière, la séparation contrôleur/service, et tout le reste du fichier.
- Supprime cette section une fois la pile fixée.
