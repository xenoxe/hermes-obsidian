# Contexte du projet

Application SaaS B2B pour le marché français : API **NestJS**, interface **Vue ou React**, **PostgreSQL partout** (local, tests, production), déploiement Docker derrière Traefik.

**Les règles complètes sont dans `AGENTS.md` à la racine du dépôt. Lis-le.** Ce fichier ne fait que rappeler l'essentiel, pour ne pas dupliquer des règles qui divergeraient.

## Avant de soumettre

- `pnpm --filter api test` et `pnpm --filter web test` passent, ainsi que `typecheck` et `lint` sur les deux.
- Les critères d'acceptation visés dans `specs/` sont couverts par un test, chemins d'erreur compris.
- Aucun `.env`, aucune clé, aucun jeton dans le diff.

## Les trois erreurs les plus coûteuses

- Une requête sur une table d'organisation sans filtre par organisation (la faille qui tue un SaaS B2B).
- Un DTO déclaré en `interface`, ou une propriété sans décorateur de validation : la validation ne s'applique qu'aux instances de classe.
- Un accès à la base depuis un contrôleur, ou depuis un composant d'interface.
