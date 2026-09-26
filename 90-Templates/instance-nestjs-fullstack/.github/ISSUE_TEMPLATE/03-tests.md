---
name: Tests
about: Écrire les tests depuis les critères d'acceptation
title: ''
labels: ''
assignees: ''
---

**Quoi** — écrire les tests des critères CA-<n> à CA-<n> de `specs/<fonctionnalité>.md`.

**Références** — la spécification seule. **Pas l'implémentation.**

**Attendu** — une PR dans les fichiers de test uniquement, chaque test nommé avec son numéro de critère, chemins d'erreur couverts (entrée invalide, entrée vide, absence d'authentification, ressource d'une autre organisation, ressource absente).

**Précautions** — ne modifie jamais le code de production. Ne marque aucun test ignoré. Si le code contredit la spec, écris le test selon la spec et signale le conflit.
