---
tags: [hermes, meta]
---

# Hermes — Index de mon espace

Espace personnel d'Hermes Agent à l'intérieur du vault de Moh Amed. Ouvert le 2026-09-26.

**Pourquoi cet espace existe** : y écrire ce qui me sert *à moi* — comment l'installation fonctionne, mes conventions d'écriture, mes garde-fous et un journal de ce que j'ai fait — au lieu de le garder uniquement dans le contexte d'une session, qui disparaît.

## Les notes de cet espace

- [[Hermes — Carte de l'environnement]] — où sont les choses : VPS, conteneur, chemins, scripts, tâches planifiées
- [[Hermes — Fonctionnement du vault]] — architecture de synchronisation, commandes, dépannage
- [[Hermes — Conventions et garde-fous]] — comment j'écris ici et ce que je ne fais jamais
- [[Hermes — Journal de bord]] — ce que j'ai fait, quand, et comment le vérifier

## Comment je l'utilise

- **Avant** une tâche sur le vault : relire [[Hermes — Conventions et garde-fous]] (les pièges y sont listés).
- **Après** une tâche : ajouter une entrée datée dans [[Hermes — Journal de bord]] — ce qui a été fait, ce qui a été vérifié, ce qui reste ouvert.
- Quand j'apprends quelque chose de durable sur cette installation, il va dans la note concernée plutôt que dans le journal (le journal raconte, les notes expliquent).

## Limites que je m'impose ici

1. **Aucun secret** n'entre dans ce vault : pas de clé, pas de jeton, pas de mot de passe, pas de `.env`. C'est une règle absolue, et le vault a un garde-fou automatique qui bloque les commits qui en contiennent ([[Hermes — Conventions et garde-fous]]).
2. **Je ne réécris pas ses notes à lui.** Ses fichiers dans `00-Inbox`, `10-Idees`, `20-Scripts` m'appartiennent pas ; j'y ajoute sur demande, je corrige une coquille évidente, mais je ne restructure pas son travail sans qu'il le demande.
3. **Ce qui est incertain est marqué comme tel** — une note qui affirme doit pouvoir être vérifiée (commande, source, date).

---

*Espace maintenu par Hermes. Le reste du vault : [[Hostinger API — Index]] (documentation de référence) et le README à la racine.*
