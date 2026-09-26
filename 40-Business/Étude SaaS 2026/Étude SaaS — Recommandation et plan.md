---
tags: [business, etude, saas, plan]
---

# Étude SaaS — Recommandation et plan

Retour : [[Étude SaaS — Index]] · [[Étude SaaS — Candidats et notation]]

## Le créneau retenu

> **Le dossier de conformité opposable de l'entreprise artisanale du bâtiment.**
> À partir du SIREN et du métier, le produit génère le calendrier de toutes les obligations à périodicité, alerte avant chaque échéance, range les procès-verbaux et attestations dans un dossier de preuve horodaté, et exporte en un clic le dossier à présenter à l'inspection du travail, à un assureur ou à un donneur d'ordre.

Promesse commerciale en une phrase : **« En cas de contrôle, vous avez le dossier complet en 30 secondes. »**

## Pourquoi ça peut devenir un leadership

1. **Le déclencheur est brutal et daté.** Depuis la loi du 25 juin 2026, un DUERP absent ou non mis à jour coûte jusqu'à **4 000 € par manquement**, multipliables par le nombre de travailleurs concernés et doublés en récidive — en plus de 7 500 € au pénal. S'ajoutent les VGP (12 mois en général, 6 mois pour les chariots élévateurs, 3 mois pour certaines nacelles), l'attestation de vigilance URSSAF à renouveler tous les 6 mois pour tout contrat ≥ 5 000 € HT, et l'émission de factures électroniques au 1er septembre 2027.
2. **Les acteurs vendent une brique — sauf un, et c'est ce qui change tout.** Duerp APP 50 €/mois **par module**, GContact 99 €/mois pour les habilitations, BatiFire 7,60-13,50 €/mois par bâtiment, Agendrop 19-29 €/mois pour les relances d'entretien, Kalindy 89 €/mois pour la sous-traitance, Provigis 650 €/an minimum : un artisan qui veut couvrir ses obligations doit acheter 3 à 5 abonnements et tenir lui-même le calendrier — c'est précisément ce qu'il ne fait pas. **Mais un acteur couvre déjà l'ensemble**, vérifié le 26/09/2026 : **Cloud VGP**, à **0,50 € HT/mois par équipement** (dégressif à 0,25 €, soit 3,60 €/équipement/an ; gratuit sous 13 500 € HT de CA), avec échéanciers VGP calés sur les textes, registre de sécurité dématérialisé, archivage horodaté exportable, veille réglementaire, Facture X et agents IA incluant le DUERP. Le créneau ne se défend donc **pas** par « personne ne fait l'ensemble » ; il se défend sur les deux choses que cet acteur ne fait pas :
   - le **versant salarié et documentaire** : un DUERP tenu comme un registre opposable (et non répondu par un agent conversationnel), le registre unique du personnel, les habilitations et leurs recyclages, la vigilance sous-traitance, les registres déchets — c'est-à-dire le champ de l'inspection du travail, pas celui de l'équipement ;
   - le **canal** de distribution, parce qu'à 30 €/mois de panier un solo ne vit que si le coût d'acquisition est proche de zéro.

   Si l'essai gratuit montre que leur DUERP est un vrai registre exportable et que leur support accompagne les TPE, la différenciation tombe : le plan B (pompes funèbres) prend alors la première place. C'est le premier test à faire, avant même les entretiens.
3. **Le canal sait déjà vendre à 30 €/mois.** Preuve vérifiée : la CAPEB distribue IArtisans à **33,28 € HT/mois** à ses adhérents, contre 49,90 € au prix public. Une fédération qui vend un abonnement logiciel, c'est exactement le coût d'acquisition proche de zéro dont un solo a besoin.
4. **La cible est la plus large du panel** : 621 803 entreprises artisanales du bâtiment, 97 % de moins de 20 salariés, et des métiers adjacents (propreté, paysage, garage, électricité) réutilisables avec la même mécanique.
5. **Ce qui est copiable ne fait pas la valeur.** Un concurrent peut recopier un écran en une semaine ; il ne peut pas recopier la base de règles par métier, les intégrations (URSSAF, contrôleurs, assureurs), ni le dossier de preuve historique d'un client — et surtout pas un accord de distribution fédéral.

## Ce que le produit fait — périmètre de la v1 (6 semaines)

**Dans la v1 :**
- Onboarding par **SIREN + code métier** → génération automatique du calendrier d'obligations
- Base de règles v1 : VGP levage et engins, extincteurs, installations électriques, chaudières/fluides frigorigènes, DUERP, SST, vigilance sous-traitance (≥ 5 000 € HT)
- Centre d'alertes par email, avec relance à J-30 / J-15 / J-7 et marquage « fait / à planifier / en retard »
- Coffre de documents : dépôt des PV et attestations, rattachement à une obligation, dates de validité
- **Export « dossier de contrôle » PDF** — la fonctionnalité qui vend
- Multi-utilisateurs (dirigeant + conjoint collaborateur + conducteur de travaux)
- Facturation Stripe, essai 14 jours sans carte

**Volontairement hors v1** (à ne pas construire, ce sont les pièges) : facturation et devis (marché saturé et gratuit), comptabilité, application mobile native, marketplace de contrôleurs, pré-remplissage automatique des documents.

## Prix

| Offre | Cible | Prix HT/mois | Prix annuel (−20 %) |
|---|---|---|---|
| Solo | 0-2 salariés | 29 € | 279 € |
| Équipe | 3-10 salariés | 49 € | 470 € |
| Chantier | 11-20 salariés | 89 € | 854 € |

- Options : pack SMS de rappel 9 €/mois · accompagnement de mise en service 190 € une fois (le tarif que pratique Agendrop, accepté par le marché)
- **Amorce commerciale** : « diagnostic de conformité » offert — un PDF personnalisé listé à partir du SIREN et du métier, produit à la main au début. C'est l'aimant à leads et la démonstration de valeur.
- Prix volontairement sous les 30 € d'entrée : la promesse est de **remplacer** 2 à 3 abonnements, pas de s'ajouter à eux.

## Dimensionnement (hypothèses explicites, pas des prévisions)

| Étape | Hypothèse | Résultat |
|---|---|---|
| Cible primaire | 30 % des 621 803 entreprises artisanales du bâtiment ont des salariés et du matériel soumis à VGP | ~186 000 entreprises |
| Cible élargie | + métiers adjacents (propreté, paysage, garage, électricité) | ~280 000 entreprises |
| Prix moyen retenu | 39 € HT/mois | 468 €/an |
| Marché adressable théorique | 186 000 × 468 € | **~87 M€/an** |
| 100 clients | mois 6 à 9 | 3 900 €/mois |
| 500 clients | mois 18 à 24 | 19 500 €/mois (≈ 234 k€/an) |
| 2 000 clients | mois 36 à 48, **impossible sans canal de distribution** | 78 000 €/mois |

Coûts : le VPS et Traefik existent déjà ; compter 100 à 400 €/mois (emails, SMS, stockage, Stripe, appels IA) → marge brute supérieure à 85 %. Le vrai coût est le temps de Moh Amed.

## Go-to-market

1. **Le canal d'abord, pas le SEO.** Cinq rendez-vous à décrocher : une CAPEB départementale, une FFB départementale, un courtier en assurance décennale ou multirisque, un cabinet d'expertise comptable spécialisé BTP, un distributeur de matériel (type Point.P, Prolians, Rexel). Argument : « nous réduisons les sinistres liés au défaut d'entretien et les non-conformités de vos adhérents / assurés / clients ».
2. **Le levier assureur est le meilleur alignement d'intérêt** : le défaut d'entretien manifeste est une cause classique d'exclusion de garantie. Un assureur qui offre l'outil économise des sinistres ; l'artisan y voit une raison de plus d'y aller.
3. **Contenu SEO par métier et par obligation** (« VGP chariot élévateur : périodicité, prix, sanction », « DUERP artisan : ce que l'inspection du travail vérifie »). Un article par obligation, un calculateur d'échéances gratuit en amont, avec capture d'email.
4. **Pendant les 6 premiers mois, vendre en direct** dans un rayon géographique limité (une région, un métier dominant) : c'est le seul moyen d'apprendre vite et d'obtenir des références locales.

## Protocole de validation en 14 jours — sans écrire une ligne de code

| Quand | Quoi | Qui fait |
|---|---|---|
| **J0 — 1 h** | **Qualifier Cloud VGP** en essai gratuit : leur « DUERP » est-il un registre exportable et daté, ou un agent conversationnel ? La vigilance URSSAF et les registres du personnel sont-ils couverts ? Qui répond au support d'un client à 5 €/mois ? | Moh Amed |
| J1-J2 | Lister 150 artisans cibles + 30 contacts de canaux. Page d'atterrissage avec les 3 offres et une pré-commande à −50 % pour les 20 premiers | Moh Amed + Hermes (liste, page, textes) |
| J3-J7 | **20 entretiens de 20 minutes** + 5 entretiens de canal. Script en 5 questions : que s'est-il passé lors de votre dernier contrôle ? qui suit vos échéances aujourd'hui ? que se passe-t-il si vous en manquez une ? combien payez-vous déjà pour ça ? quel serait le montant acceptable ? | Moh Amed (l'entretien ne se délègue pas) |
| J8-J10 | **Démonstration à la main** : pour 10 volontaires, produire leur calendrier d'obligations et leur dossier de conformité PDF à partir du SIREN, sans produit | Hermes (recherche + génération) |
| J11-J14 | Demander **3 pré-commandes payantes** (49 € pour 6 mois) et **1 lettre d'intention de canal** | Moh Amed |

**Critères d'arrêt (si l'un est vrai, on ne construit pas) :**
1. Moins de 5 artisans sur 20 confirment avoir déjà subi ou craindre un contrôle avec des documents manquants
2. Zéro pré-commande payante en 14 jours (les « c'est intéressant » ne comptent pas)
3. Découverte d'un acteur qui couvre déjà l'ensemble à 39 €/mois ou moins
4. Zéro canal intéressé après 5 rendez-vous — auquel cas le modèle à 30 €/mois ne tient pas avec un solo
5. **Cloud VGP (ou équivalent) couvre le DUERP comme un registre exportable, la vigilance et les registres du personnel, avec un support joignable pour les TPE** → la différenciation retenue tombe, on bascule sur le plan B sans chercher à rivaliser sur le prix

**Si le test est vert** : construire la v1 en 6 semaines, objectif 10 clients payants en bêta, puis 100 clients en 6 mois.

## Plan B et plan C

- **Plan B — pompes funèbres** (26/40) : ARPU élevé, éditeurs vieux, aucun prix public, marché en croissance jusqu'en 2040. À activer si le canal fédéral ne fonctionne pas sur le bâtiment. Prévoir un cycle de vente long et de la vente en présentiel.
- **Plan C — auto-écoles** (29/40) : marché prouvé et solvable (99 à 249 €/mois chez Klaxo) mais six entrants récents se battent déjà et le choc CPF de février 2026 (permis léger réservé aux demandeurs d'emploi, plafond 900 €) serre les budgets. À jouer seulement sur un angle que personne n'a pris : le **pilotage des financements** (reste à charge, cofinancements, France Travail, Opco), pas le planning.

## Risques principaux et parades

| Risque | Parade |
|---|---|
| **Un acteur couvre déjà l'ensemble moins cher que mon offre** (Cloud VGP, 0,50 €/équipement/mois, gratuit sous 13 500 € de CA) | Ne jamais se battre sur le prix : se différencier sur le registre salarié et documentaire, et sur le canal. Leur API est ouverte — l'intégration (être la couche « inspection du travail » branchée sur leur parc) est une option moins risquée que la concurrence frontale, et elle se teste en un appel |
| Les acteurs par brique (Duerp APP, GContact, BatiFire) étendent vers l'échéancier global | Verrouiller le canal et la profondeur par métier avant eux ; viser les centrales d'achat et les fédérations |
| La CAPEB donne le DUERP gratuitement et pourrait donner l'échéancier | Ne jamais vendre le DUERP : vendre la **preuve** et le **dossier de contrôle** multi-obligations, que les outils gratuits ne produisent pas |
| Les artisans n'achètent pas par conviction, seulement sous contrainte | Vendre juste après un déclencheur : contrôle, refus de chantier, refus d'attestation, sinistre, mise en demeure |
| Churn : fermeture d'entreprise, changement d'activité | Contrat annuel avec −20 %, et rattacher la valeur au dossier (l'historique de preuves se conserve, donc se renouvelle) |
| Erreur de règle → responsabilité juridique | Sourcing public et daté de chaque règle, avertissement clair dans le produit, aucune interprétation : on affiche la périodicité légale, pas un conseil |
| Un solo ne peut pas tenir le support de milliers de comptes | Self-serve dès la 100ᵉ vente, documentation, réponses type, et le canal absorbe le premier niveau |

## Ce que Hermes peut prendre en charge

- **Déploiement** de l'application sur le VPS existant, en HTTPS via Traefik, avec l'API Hostinger (voir [[Hostinger API — VPS]])
- **Veille réglementaire automatisée** : tâche planifiée qui surveille les sources (Légifrance, INRS, service-public, URSSAF, CAPEB) et signale toute modification de périodicité ou de sanction — c'est-à-dire la matière première de la base de règles
- **Production des diagnostics de conformité à la main** pendant la phase de test (J8-J10 du protocole)
- **Contenu SEO** : un article par obligation et par métier, avec les sources officielles
- **Préparation des entretiens** : liste de prospects qualifiés, script, grille de dépouillement des 20 réponses

## La phrase à retenir

Personne n'a fait « ce SaaS » parce que ce n'est pas *un* logiciel : c'est **une obligation datée + une preuve opposable + un canal fédéral**. Les trois ensemble ne sont occupés par personne, et c'est exactement la combinaison qu'un solo technique peut verrouiller en 18 mois.
