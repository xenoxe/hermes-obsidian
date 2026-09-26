> **Modèle à copier dans un dépôt de code.** Ce fichier ne s'applique pas au vault où il est rangé : il est destiné à être copié à la racine d'un projet TypeScript, sous le nom `AGENTS.md`.

# <Nom du SaaS>

Application SaaS B2B pour le marché français (TPE, artisans, PME). Abonnement en revenu récurrent.

**Pile** (à ajuster si la tienne diffère — voir en fin de fichier) :
Node 22 · TypeScript strict · Next.js 15 (App Router) · React 19 · PostgreSQL 16 · Prisma · Zod · Vitest · Playwright · pnpm · ESLint + Prettier.

**Points d'entrée** : `app/` (pages et routes d'API) · `server/` (logique métier et accès aux données) · `prisma/schema.prisma` (modèle de données) · `middleware.ts` (authentification et multi-tenant).

## Commandes

```bash
pnpm install                       # installer
pnpm dev                           # serveur de développement
pnpm build                         # construction de production
pnpm test                          # tous les tests unitaires et d'intégration
pnpm vitest run <chemin>           # UN seul fichier de test  ← la plus utilisée
pnpm vitest run -t "<nom du test>" # un seul test par son nom
pnpm test:e2e                      # tests de bout en bout (Playwright)
pnpm test:e2e -- <chemin>          # un seul scénario de bout en bout
pnpm lint                          # ESLint (pnpm lint --fix pour corriger)
pnpm typecheck                     # tsc --noEmit
pnpm format                        # Prettier

pnpm prisma migrate dev --name <nom>   # nouvelle migration (développement)
pnpm prisma migrate deploy             # appliquer en production
pnpm prisma generate                   # régénérer le client typé
pnpm prisma studio                     # explorer la base en local

docker build -t <image>:<tag> .        # image de déploiement
```

**Toujours lancer `pnpm test` ET `pnpm typecheck` avant de soumettre.** Un test lancé par soi-même, pas rapporté de mémoire.

## Architecture

- `app/` — pages et routes d'API (App Router). Une route d'API vit dans `app/api/<ressource>/route.ts`.
- `app/(app)/` — zone authentifiée ; `app/(public)/` — zone publique.
- `components/` — composants d'interface réutilisables. Aucun accès aux données ici.
- `server/` — logique métier, services, accès aux données. **Tout accès à la base passe par ici.**
- `lib/` — utilitaires purs, sans effet de bord, sans dépendance à la base.
- `prisma/` — schéma et migrations versionnées.
- `tests/` — tests unitaires et d'intégration (miroir de l'arborescence de `server/` et `lib/`).
- `e2e/` — scénarios Playwright.
- `scripts/` — scripts d'exploitation (sauvegarde, import, maintenance).
- Ne pas créer de nouveau dossier de premier niveau sans le demander.

## Conventions

- **TypeScript strict.** `any` est interdit. `unknown` puis validation, ou un type précis. Les assertions `as` sont un dernier recours, jamais une commodité.
- **Exports nommés** uniquement. Pas d'export par défaut, sauf exigence d'un cadre (pages Next.js, `route.ts`).
- **Validation des entrées par Zod à la frontière** : route d'API, action serveur, formulaire, webhook, variable d'environnement, réponse d'un service tiers. Jamais au fond de la logique métier.
- **Les erreurs attendues sont des exceptions typées.** On ne renvoie jamais `null` pour signaler un échec, et on n'attrape pas une erreur pour ne rien faire.
- **Aucun accès direct à la base depuis un composant ou une page.** Route d'API → service dans `server/` → Prisma.
- Un composant fait une chose. Au-delà de ~150 lignes, il se découpe.
- **Identifiants en anglais, textes affichés en français.** Pas de texte en dur dans les composants : tout passe par la table de traduction.
- **Dates stockées en UTC (ISO 8601)**, affichées en `jj/mm/aaaa`.
- **Montants en centimes entiers**, jamais en flottant. Un prix en base est un `integer`.
- Ne pas reformater un fichier qu'on n'a pas modifié : cela rend le diff illisible.
- Nommage : `dossier-en-kebab-case/`, `ComposantEnPascalCase.tsx`, `fonctionEnCamelCase.ts`, `TABLE_EN_SNAKE_CASE`.

## Base de données et migrations

- **Une migration déjà appliquée ne se modifie jamais.** On en écrit une nouvelle.
- Toute migration touchant une colonne existante est **réversible** (ou accompagnée de son plan de retour écrit dans la description de la PR).
- Une migration destructive (suppression de colonne ou de table, changement de type) **se demande avant** — jamais en autonomie, même en développement.
- Aucune commande Prisma pointant vers la production lancée à la main.
- Le schéma est la référence du modèle de données : un changement de comportement qui ne s'y reflète pas est un oubli.

## Multi-tenant et sécurité

- **Toute requête touchant des données d'organisation est filtrée par l'identifiant d'organisation.** Une requête non filtrée sur une table d'organisation est un défaut critique, pas un oubli mineur. C'est la faille qui tue un SaaS B2B.
- Chaque route et chaque action serveur vérifie que l'appelant est authentifié **et** autorisé sur la ressource visée. Une autorisation manquante est critique.
- Les secrets vivent dans l'environnement. On les nomme par leur variable (`DATABASE_URL`, `STRIPE_SECRET_KEY`), jamais par leur valeur. Aucun secret dans le dépôt, dans un test, dans un commentaire ou dans un fichier d'exemple.
- Les webhooks vérifient leur signature avant de traiter quoi que ce soit, et sont idempotents.
- Aucune donnée personnelle dans les journaux. Les messages d'erreur ne révèlent pas l'intérieur du système à l'utilisateur.
- Mots de passe hachés avec un algorithme à jour (argon2 ou bcrypt), jamais stockés ni journalisés.
- Données personnelles : on ne collecte que le nécessaire, et tout ce qui est collecté doit pouvoir être exporté et supprimé à la demande (RGPD).
- Toute dépendance ajoutée doit être connue, maintenue et nécessaire.

## Tests

- Cadre : **Vitest** pour l'unitaire et l'intégration, **Playwright** pour le bout en bout.
- Un test par comportement, nommé par le comportement attendu (`rejette une facture sans client`), jamais par le nom de la fonction (`test factureService`).
- Les tests en base utilisent la **base de test** (`docker compose up -d db-test`), jamais la base de développement.
- **Couvrir les chemins d'erreur**, pas seulement le cas nominal : entrée invalide, entrée vide, absence d'authentification, accès à une ressource d'une autre organisation, ressource absente, quota dépassé.
- Trois scénarios de bout en bout sont obligatoires et doivent rester verts : inscription, connexion, première souscription.
- Les tests existants doivent continuer de passer. Un test qu'on doit modifier mérite une explication dans la PR.
- On n'affaiblit jamais un test, on ne le marque pas ignoré, on ne désactive pas une vérification de types pour faire passer la suite.

## Facturation

- Montants en **centimes entiers**. Aucun calcul monétaire en flottant.
- Toute opération de facturation est **idempotente** (clé d'idempotence), parce qu'un webhook arrive parfois deux fois.
- L'état de l'abonnement fait foi côté fournisseur de paiement ; on ne le déduit pas d'un calcul local.
- Toute modification touchant les prix, les quotas, les périodes d'essai ou les remises se **demande avant**.
- Les événements de facturation sont journalisés sans données de carte ni jeton.

## Déploiement

- Conteneur Docker, servi derrière **Traefik en HTTPS** sur le VPS. Pas de port publié à la main, pas de HTTP brut.
- L'image de production ne contient pas les dépendances de développement ; les secrets arrivent par variables d'environnement, jamais dans l'image.
- Une variable d'environnement nouvelle est ajoutée au fichier d'exemple **sans sa valeur**, et signalée dans la PR — sinon le déploiement suivant échoue en silence.

## Frontières

### Toujours
- `pnpm test` et `pnpm typecheck` passent, lancés par celui qui l'affirme.
- Respecter l'arborescence et les motifs existants ; lire les fichiers voisins avant d'écrire.
- Committer sur sa propre branche, un sujet par branche.
- Nommer dans le rapport final : fichiers touchés, commandes lancées, résultat réel.

### Demander avant
- Modifier le schéma de base de données ou écrire une migration destructive.
- Ajouter une dépendance, même petite, même « évidente ».
- Modifier le contrat d'API public, une route existante, ou la forme d'une réponse.
- Toucher à l'authentification, à l'autorisation, aux jetons, aux webhooks, à la facturation, aux quotas.
- Modifier la CI, les fichiers de configuration partagés, le `Dockerfile`, la configuration Traefik.
- Ajouter un service externe ou une variable d'environnement nouvelle.

### Jamais
- Lire, écrire ou recopier un `.env`, une clé, un jeton, un certificat, une chaîne de connexion. Pas même dans un test ou un commentaire.
- Écrire une requête sur une table d'organisation sans filtre par organisation.
- Modifier une migration déjà appliquée, ou lancer une commande de base de données pointant vers la production.
- Utiliser `any`, désactiver une règle ESLint ou un contrôle de types pour faire passer la construction.
- `git push --force`, réécrire l'historique, supprimer une branche, fusionner sans relecture humaine.
- Désactiver, ignorer ou affaiblir un test.
- Introduire un calcul monétaire en flottant.

## Définition de terminé

Une tâche est terminée quand :
`pnpm test` et `pnpm typecheck` passent (lancés par l'agent qui l'affirme) ·
`pnpm lint` passe sans avertissement nouveau ·
les critères d'acceptation visés sont couverts par un test ·
les chemins d'erreur sont couverts, pas seulement le cas nominal ·
aucune frontière n'a été franchie ·
aucun secret, aucune donnée personnelle dans les journaux ou le diff ·
la PR nomme les fichiers touchés, les commandes lancées et ce qui reste incomplet.

## Format du rapport final

```
Livré      : <ce qui fonctionne maintenant>
Critères   : <CA couverts, CA non couverts>
Commandes  : <commande> → <résultat réel>
Incomplet  : <ce qui reste, et pourquoi>
Divergence : <tout écart avec la spécification>
```

Ne jamais affirmer qu'un test passe sans l'avoir lancé. « Devrait fonctionner » n'est pas un résultat.

## Si ta pile diffère

Trois endroits seulement portent des choix de pile, à ajuster puis supprimer cette section :

1. le bloc **Pile** en tête, et les **Commandes** (gestionnaire de paquets, cadre de test, outil de migration) ;
2. l'**Architecture**, si l'arborescence n'est pas celle de Next.js ;
3. la section **Déploiement**, si ce n'est pas Docker derrière Traefik.

Le reste — conventions, multi-tenant, tests, facturation, frontières, définition de terminé — ne dépend pas du cadre et se garde tel quel.
