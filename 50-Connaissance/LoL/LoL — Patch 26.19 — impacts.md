---
tags: [lol, patch, impacts]
patch: "26.19"
date_verification: 2026-09-27
---

# LoL — Patch 26.19 — impacts

Date de vérification : 2026-09-27 · Patch live : 26.19 (Data Dragon 16.19.1) · Sorti le 2026-09-23 (notes FR publiées le 22)
Compte : **x3nøx3#EUW**, EUW, Silver 1 (32 V / 31 D, 66 LP) · Rôle : jungle
Voir aussi : [[LoL — Veille patches]]

## Ce qui change pour toi

- **Master Yi** (5 parties, 60 %, KDA 2,19) — *Alpha Strike* : la réduction de recharge gagnée sur les attaques n'est plus réduite par la hâte de compétence ; l'ancienne valeur chiffrée n'est pas donnée dans les notes → **non mesuré**. Conséquence concrète : en combat prolongé, tu récupères Q moins souvent qu'avant si tu as de la hâte dans ton build — le pic de dégâts en escarmouche baisse, pas le burst d'entrée.
- **Lillia** (4 parties, 50 %, KDA 2,54) — armure de base **22 → 24** et *Berceuse* : durée de sommeil **2,0 s → 2,5 s**. Buff net : clear plus confortable face aux junglers AD et fenêtre de kill allongée sur R.
- **Shyvana** (18 parties, 56 %, KDA 3,23), **Briar** (8 parties, 63 %, KDA 2,36), **Warwick** (5 parties, 40 %, KDA 2,06) : **aucun changement** en 26.19 (liste des champions modifiés relue en entier).
- **Ta voie** : aucun objet de jungle touché. Les deux items modifiés (World Atlas, Runic Compass) sont des items de support — hors de ta voie. La quête de voie du haut passe Téléportation de **420 s à 390 s** : ça ne change pas ton jeu, mais les tops arrivent en bot lane un peu plus tôt qu'avant.

## Le reste de la jungle (pas dans ton pool, pour info seulement)

- Kha'Zix : *Menace invisible* **17-136 → 22-141**, Q évolué ralentit **2 → 2,25 s**, E évolué portée **200 → 300**
- Elise : *Reine araignée* **12-42 → 14-44**, *Frénésie* **60-120 % → 70-130 %** de vitesse d'attaque
- Poppy : Q plafonné sur les monstres **85-225 → 70-210** (clear plus lent)
- Rumble : plafond monstres **65-150 → 90-175** (clear plus rapide) mais vitesse d'attaque en surchauffe **50-130 % → 30-100 %**
- Nocturne nerfé : R **140-90 s → 160-100 s** · Vi mitigée : AD de base **63 → 61** mais AD par niveau **3,5 → 3,9**, bouclier **12 % → 10 %** des PV max

## Ce que tu joues cette semaine

- **Briar en premier choix** : 52,49 % de winrate (370 737 parties, tier S, rang 3/79) et c'est ton meilleur taux personnel sur le mois (63 % sur 8 parties). Rien n'a bougé dessus.
- **Shyvana en filet de sécurité** : 52,92 % (69 437 parties, A-, rang 24/79) — c'est ton plus gros volume de jeu (18 parties, 56 %) et elle est intacte.
- **Lillia : le bon choix face à un Master Yi ennemi** — 51,87 % de winrate (129 882 parties, A+, rang 13/82), buffée sur deux lignes (armure de base, sommeil de R). lolalytics la classe parmi les championnes les plus battues par Master Yi, donc si l'adversaire prend Yi, c'est une réponse chiffrée.
- **Master Yi à surveiller** : 50,11 % (734 895 parties) — le winrate le plus bas de ton pool, avec un nerf qui vient de tomber. Ne le prends pas par défaut cette semaine.

## Ce que tu arrêtes (temporairement)

- **Warwick : 40 % sur 5 parties**, aucun buff en 26.19 → hors du pool pour deux semaines (jusqu'au patch 26.20, ~2026-10-07). Il reste listé comme counter de Master Yi si tu dois le reprendre en réponse.
- **Aucun item ni rune de ton build n'est touché** : pas de raison de changer une page de runes cette semaine. Un changement de build au patch day, sans données, coûte des LP.

## Le point chiffré de la semaine

Sur tes **17 dernières parties classées Solo/Duo** (deeplol, relevé le 2026-09-27) : 9 V / 8 D, **5,82 morts par partie** en moyenne.
Victoires : **4,56 morts** en moyenne · Défaites : **7,25 morts** en moyenne, dont 2 des 8 défaites à 10 morts ou plus.
**Objectif de la semaine : ≤ 5 morts par partie.** C'est la variable qui sépare tes victoires de tes défaites sur cet échantillon, bien plus que le contenu du patch.

## Ce qui n'est pas vérifié

- **Patch trop frais pour les taux** : sorti le 23/09, relevé le 27/09 → 4 jours de données. Les winrates ci-dessus sont indicatifs, la stabilisation demande 3 à 7 jours.
- **Étiquetage de patch incohérent sur lolalytics** : la page tier list affiche « PATCH 16.19 » tandis que le texte des fiches champions affiche encore « Patch 16.11 ». Chiffres relevés sur l'onglet *All Ranks / jungle* le 2026-09-27 — l'origine exacte de l'échantillon n'a pas pu être confirmée.
- **Aucune donnée API Riot ce jour-là** : la clé de développement est absente ou expirée (`riot_fetch.py probe` → « RIOT_API_KEY absente »). La maîtrise par champion n'est donc **pas mesurée** ; le pool et le rang viennent de deeplol.gg (source tierce).

## Sources

- wiki LoL, V26.19 (contenu chiffré des notes) : https://wiki.leagueoflegends.com/en-us/V26.19 (consulté le 2026-09-27)
- Notes officielles FR 26.19 : https://www.leagueoflegends.com/fr-fr/news/game-updates/league-of-legends-patch-26-19-notes/ (publiées le 2026-09-22)
- Patch live : Data Dragon `versions.json` → 16.19.1 (= 26.19), lu via `riot_fetch.py versions` le 2026-09-27
- Winrates : lolalytics.com, onglet All Ranks, voie jungle, par champion (shyvana, briar, warwick, masteryi, lillia), relevé le 2026-09-27
- Profil et historique : deeplol.gg, `x3nøx3#EUW`, relevé le 2026-09-27 — **source tierce, non confirmée par l'API Riot**
