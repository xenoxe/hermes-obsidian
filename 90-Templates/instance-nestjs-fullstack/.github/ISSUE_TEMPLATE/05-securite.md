---
name: Sécurité
about: Auditer la surface d'attaque d'une pull request
title: ''
labels: ''
assignees: ''
---

**Quoi** — auditer la surface d'attaque du diff de la PR #<n>.

**Références** — la PR, `AGENTS.md` (frontières).

**Attendu** — un verdict et un rapport par sévérité ; chaque constat critique accompagné d'un scénario d'exploitation concret (qui peut faire quoi, et ce qu'il obtient).

**Précautions** — ne modifie aucun code. N'oublie aucune entrée de frontière, ni les secrets en dur dans les tests, ni les requêtes non filtrées par organisation.
