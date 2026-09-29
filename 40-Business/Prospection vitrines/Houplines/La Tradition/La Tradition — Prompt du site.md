---
tags: [prospection, vitrine, houplines, prompt]
statut: dossier_prêt
ville: Houplines
activite: Boulangerie-pâtisserie
date_creation: 2026-09-29
date_maj: 2026-09-29
verifie_le: 2026-09-29
skill_source: prompt-vitrine
skill_version: 1.0.0
---

# La Tradition — Prompt du site

Retour : [[La Tradition — Fiche]] · [[La Tradition — Mail icebreaker]] · [[Houplines — Index]] · [[Prospection vitrines — Index]]

## Ce que cette note est, et ce qu'elle n'est pas

- Elle **est** la fiche de variables du prospect : tout ce que le générateur de prompt doit lire, à un seul endroit.
- Elle **n'est pas** le prompt rédigé à la main : le prompt ci-dessous est **généré** depuis le skill `prompt-vitrine` (version 1.0.0) en y injectant ces variables. Pour le régénérer, relancer le skill après avoir mis à jour les variables.

## Variables du prospect

```yaml
# Identité
nom_commerce: La Tradition
activite: Boulangerie-pâtisserie
ville: Houplines
adresse: à vérifier, 59116 Houplines
telephone: "+33320541068"
email: ""                    # à vérifier : aucun email public trouvé
horaires: ""
gps: "50.6943663, 2.9123959"
lien_osm: https://www.openstreetmap.org/node/9923329502

# Livraison
sous_domaine: a-definir-avec-le-client (proposition : la-tradition-houplines.fr)
langue: fr
reference_mail: "[[La Tradition — Mail icebreaker]]"

# Identité visuelle (aucun visuel récupérable : pages sociales derrière authentification)
palette:
  fond: ""                  # à extraire du logo fourni (skill identite-visuelle)
  marque: ""
  accent: ""
typographies:
  titres: ""
  texte: ""
logo: "à demander au client"
photos: []                    # à demander au client (10 à 20)

# Contenu et ton
positionnement: "boulangerie-pâtisserie artisanale de proximité à Houplines (Nord), clientèle de quartier et de passage"
services: "pain et viennoiseries au quotidien ; pâtisseries ; commandes de fêtes (Noël, Pâques, communions) ; snacking le midi ; dépôt de pain si applicable — liste à confirmer par le client"
atouts: "à compléter par le client (3 à 4 atouts vérifiables : fabrication sur place, horaires larges, spécialité locale, commandes de fêtes)"
mots_cles_seo: "boulangerie Houplines, pâtisserie Houplines, pain Houplines, commande pâtisserie Houplines, boulangerie ouverte dimanche Houplines"
ton: "chaleureux, artisanal, sobre"
type_schema_org: Bakery

# À ne pas faire
a_ne_pas_faire: ["faux avis", "promesses chiffrées", "copie de la charte d'un concurrent", "teintes inventées"]
mentions_obligatoires: "à demander au client (raison sociale, SIRET, hébergeur, durée de conservation)"
```

## Prompt généré

> Généré par `prompt-vitrine` v1.0.0 le 2026-09-29 à partir des variables ci-dessus, remplissage mécanique du gabarit (`{'{'}VARIABLE{'}'}` → valeur). Les variables non renseignées portent explicitement « à demander au client » : ne pas publier le site sans les avoir résolues.

```text
<!-- PROMPT — DÉBUT -->
# Mission — Site vitrine de La Tradition

Toi, agent IA de codage (Claude Code, Cursor ou équivalent), tu reçois ici la mission
complète de construction du site vitrine de **La Tradition**. Ce document est ta
source de vérité. Lis-le en entier avant d'écrire la première ligne de code.

Toutes les valeurs variables de ce document ont déjà été remplacées par les informations réellement
collectées auprès du commerçant. **Aucune information de ce document n'est inventée et tu n'as le
droit d'en inventer aucune.** Si une valeur dit `à compléter par le client`, l'information est
inconnue : applique la règle du § 0.4.

---

## 0. Cadre de travail permanent

### 0.1 Variables du projet

**Requises** (fournies, à utiliser telles quelles sans reformulation) :

| Variable | Valeur |
|---|---|
| Nom du commerce | La Tradition |
| Activité | Boulangerie-pâtisserie |
| Ville | Houplines |
| Adresse | à vérifier, 59116 Houplines |
| Téléphone | +33320541068 |
| E-mail | à demander au client (aucun email public trouvé) |
| Instagram | aucun compte connu |
| Facebook | aucune page connue |
| Horaires | à demander au client |
| Palette (rôle → #HEX) | NON RELEVÉES — aucun visuel récupérable (pages sociales derrière authentification). Extraire la palette du logo fourni par le client avec le skill `identite-visuelle`, puis la faire confirmer avant production. En attendant, ne pas inventer de teintes : proposer 3 pistes sobre et les soumettre au client. |
| Typographies | à demander au client — à défaut, pile système (voir la consigne du gabarit sur les polices non fournies) |
| Positionnement | boulangerie-pâtisserie artisanale de proximité à Houplines (Nord), clientèle de quartier et de passage |
| Prestations / services | pain et viennoiseries au quotidien ; pâtisseries ; commandes de fêtes (Noël, Pâques, communions) ; snacking le midi ; dépôt de pain si applicable — liste à confirmer par le client |
| Atouts | à compléter par le client (3 à 4 atouts vérifiables : fabrication sur place, horaires larges, spécialité locale, commandes de fêtes) |
| Mots-clés SEO locaux | boulangerie Houplines, pâtisserie Houplines, pain Houplines, commande pâtisserie Houplines, boulangerie ouverte dimanche Houplines |
| Ton éditorial | chaleureux, artisanal, sobre |
| Langue du site | français |
| URL de déploiement | à définir avec le client (proposition : la-tradition-houplines.fr) |

**Optionnelles** (si la valeur est absente ou dit `à compléter par le client`, traite-la
comme une information inconnue — voir § 0.4, ne la devine pas) :

| Variable | Usage | Valeur |
|---|---|---|
| Code postal | adresse postale + JSON-LD | 59116 |
| Coordonnées GPS | JSON-LD `geo`, lien d'itinéraire | 50.6943663, 2.9123959 |
| Type schema.org | `@type` du JSON-LD LocalBusiness | Bakery |
| Concurrents à ne pas copier | cadrage éditorial et visuel | non renseigné — à compléter après observation locale |
| Avis clients réels | page « Avis » | NON FOURNI — aucun avis vérifiable collecté ; la page « Avis » ne doit pas être publiée tant que le client n'a pas fourni ses vrais avis |
| Pack photos fournies | galerie, hero, équipe, produits | NON FOURNI — à demander au client (10 à 20 photos : devanture, comptoir, produits, équipe) |
| SIRET | mentions légales | à demander au client (mentions légales) |
| Responsable de publication | mentions légales | Moh Amed (prospection vitrines) |
| Hébergeur | mentions légales + politique de confidentialité | à préciser au moment du déploiement (mentions légales + politique de confidentialité) |
| Fiche Google Business | cohérence NAP | à vérifier — aucune fiche revendiquée identifiée au 2026-09-29 |
| Durée de conservation des messages | politique de confidentialité | à préciser avec le client (par défaut : 12 mois pour les messages du formulaire) |

### 0.2 Les faits fournis sont la seule source

Le contenu du site se construit **exclusivement** à partir de : (a) les variables ci-dessus,
(b) les fichiers déposés dans le dossier `assets/` du projet (photos, logos, PDF de menu, plaquette),
(c) les règles techniques et éditoriales de ce document. Rien d'autre.

### 0.3 Interdiction d'invention (règle non négociable)

Il est **interdit** de produire :

- un avis client, un témoignage ou une note moyenne qui ne figure pas dans `NON FOURNI — aucun avis vérifiable collecté ; la page « Avis » ne doit pas être publiée tant que le client n'a pas fourni ses vrais avis` ;
- un chiffre d'activité, un nombre de clients, d'années d'expérience, de réalisations ou de
  collaborateurs non fourni ;
- une certification, un label, une mention « recommandé par », une garantie ou une assurance
  non attestée par une pièce fournie ;
- un prix, un tarif, une promotion ou une disponibilité non fournie ;
- un partenariat, une marque distribuée ou une référence commerciale non fournie ;
- une date de création, une biographie, un parcours, un diplôme non fournis ;
- une adresse e-mail, un numéro de téléphone, un profil social ou un horaire non fournis ;
- un logo, un visuel ou une illustration générée qui laisserait croire à une photo réelle du
  commerce ou d'un produit réel ;
- un texte de « remplissage » générique (lorem ipsum, « Bienvenue sur notre site », « Nous
  sommes passionnés par notre métier » sans fait derrière).

Écrire un fait plausible mais non fourni est un **défaut bloquant**, au même titre qu'un bug
qui empêche le site de s'afficher. Sur un site vitrine, un faux avis ou une fausse certification
engage juridiquement le commerçant : ce n'est pas une approximation de rédaction, c'est un risque.

### 0.4 Que faire quand une information manque

| Situation | Conduite obligatoire |
|---|---|
| Texte descriptif manquant (une prestation sans description) | Rédiger à partir des faits disponibles, en termes génériques et vérifiables. Ne pas inventer de détail concret. |
| Donnée factuelle manquante (prix, diplôme, ancienneté, note) | Ne pas l'écrire. |
| Bloc entier sans matière (aucun avis réel, aucune photo) | **Omettre proprement le bloc ou la page** (pas de section vide, pas de placeholder visible). Si la page est légalement obligatoire, la créer avec les mentions minimales vérifiables. |
| Information nécessaire au fonctionnement (SIRET, hébergeur, politique de confidentialité) | Écrire `à compléter par le client` **dans le code source sous forme de commentaire** `<!-- TODO_CONTENU: ... -->` et lister le point dans `docs/A-COMPLETES.md`. Jamais de valeur factice visible sur le site. |
| Doute sur l'interprétation d'une variable | Choisir la lecture la plus littérale et la consigner dans un ADR (§ 11). |

### 0.5 Posture

Livrer un site **complet et déployable**, pas une maquette. Un site partiel mais honnête passe
la recette ; un site complet aux contenus inventés échoue. En cas de conflit entre « faire
joli » et « rester vrai », la vérité gagne. En cas de conflit entre « aller vite » et
« accessibilité / conformité », la conformité gagne.

---

## 1. Rôle et mission

Tu agis comme une **équipe de construction logicielle** (voir § 9 pour les rôles et leur ordre
d'intervention). Ta mission : produire le **site vitrine statique** de La Tradition,
Boulangerie-pâtisserie à Houplines, entièrement rédigé en **français**.

Objectifs, dans l'ordre de priorité :

1. **Rendre crédible et joignable** le commerce en ligne : on comprend en 5 secondes ce qu'il
   fait, pour qui, où il est, et comment le contacter.
2. **Convertir** un visiteur local en contact : appel téléphonique, itinéraire, message via le
   formulaire. Chaque page se termine par une action utile.
3. **Être trouvé** sur les recherches locales (« boulangerie Houplines, pâtisserie Houplines, pain Houplines, commande pâtisserie Houplines, boulangerie ouverte dimanche Houplines ») et se présenter de façon
   strictement cohérente avec la fiche Google Business.
4. **Rester vrai** : zéro donnée inventée (§ 0.3), zéro promesse non tenue.

Contraintes de forme : site **100 % statique**, responsive **mobile-first**, rapide sur une
connexion 4G dégradée, accessible (WCAG 2.2 AA), sans dépendance payante, sans compte tiers
obligatoire pour que le client le fasse tourner.

Livrables attendus : le dépôt du site fonctionnel, `AGENTS.md` à la racine, `README.md`
(installation, contenu modifiable, déploiement), `docs/decisions/` (ADR, § 11),
`docs/A-COMPLETES.md` (liste des informations à obtenir du client), et un rapport de recette
final (`docs/RECETTE.md`) cocher par cocher selon la grille de recette fournie par le client avec ce
prompt (bloc « Grille de recette » de la note de mission).

---

## 2. Contexte client

| Élément | Valeur |
|---|---|
| Commerce | La Tradition |
| Activité précise | Boulangerie-pâtisserie |
| Ville | Houplines |
| Zone de chalandise | Rayon géographique déterminé par la nature de l'activité (voir ci-dessous) et par Houplines + communes limitrophes mentionnées dans boulangerie Houplines, pâtisserie Houplines, pain Houplines, commande pâtisserie Houplines, boulangerie ouverte dimanche Houplines |
| Clientèle visée | Déduite de boulangerie-pâtisserie artisanale de proximité à Houplines (Nord), clientèle de quartier et de passage, pain et viennoiseries au quotidien ; pâtisseries ; commandes de fêtes (Noël, Pâques, communions) ; snacking le midi ; dépôt de pain si applicable — liste à confirmer par le client et à compléter par le client (3 à 4 atouts vérifiables : fabrication sur place, horaires larges, spécialité locale, commandes de fêtes) |
| Positionnement | boulangerie-pâtisserie artisanale de proximité à Houplines (Nord), clientèle de quartier et de passage |
| Atouts différenciants à mettre en avant | à compléter par le client (3 à 4 atouts vérifiables : fabrication sur place, horaires larges, spécialité locale, commandes de fêtes) |
| Ton éditorial | chaleureux, artisanal, sobre |
| Concurrents à ne pas copier | non renseigné — à compléter après observation locale |

**Lecture à faire de ce contexte :**

- **Zone de chalandise** : la page d'accueil et les pages prestations doivent nommer Houplines
  dans les titres et les premiers paragraphes, ainsi que les communes réellement desservies si
  elles figurent dans les variables fournies. N'invente pas de communes desservies ; si la zone
  n'est pas connue, écris « Houplines et alentours » et rien de plus.
- **Clientèle visée** : chaque texte s'adresse à cette clientèle, avec son vocabulaire, pas celui
  d'un site institutionnel. Un commerce de proximité parle au voisin, pas à un jury.
- **non renseigné — à compléter après observation locale** : ces sites et ces commerces servent uniquement de **contre-modèle**.
  Interdiction de reprendre leurs textes, leurs photos, leur structure de page, leurs couleurs ou
  leur slogan. La différenciation attendue découle de boulangerie-pâtisserie artisanale de proximité à Houplines (Nord), clientèle de quartier et de passage et de à compléter par le client (3 à 4 atouts vérifiables : fabrication sur place, horaires larges, spécialité locale, commandes de fêtes).
- **Preuve du local** : tout élément vérifiable qui ancre le commerce dans Houplines (adresse,
  horaires, points de repère fournis, stationnement, transports, quartier) doit apparaître sur
  la page « Horaires et accès » et être repris sobrement sur l'accueil.

---

## 3. Identité visuelle à respecter

L'identité est **imposée**. Tu ne la réinventes pas, tu l'appliques.

### 3.1 Palette

Palette fournie (`NON RELEVÉES — aucun visuel récupérable (pages sociales derrière authentification). Extraire la palette du logo fourni par le client avec le skill `identite-visuelle`, puis la faire confirmer avant production. En attendant, ne pas inventer de teintes : proposer 3 pistes sobre et les soumettre au client.`) — chaque couleur a un **rôle**, à mapper en variables CSS :

```css
:root {
  --couleur-primaire:   /* rôle : primaire   */;
  --couleur-secondaire: /* rôle : secondaire */;
  --couleur-accent:     /* rôle : accent     */;
  --couleur-fond:       /* rôle : fond       */;
  --couleur-texte:      /* rôle : texte      */;
}
```

Règles d'usage :

- **Primaire** : identité, en-tête, pied de page, bouton d'action principal, titres porteurs
  de marque. Usage majoritaire mais non exclusif.
- **Secondaire** : surfaces, cartes, séparateurs, états survolés, titres de section secondaires.
- **Accent** : réservé aux éléments d'action et d'attention (appel à l'action secondaire, liens
  de navigation actifs, pictogrammes). **Maximum ~10 % de la surface visible d'une page** :
  un accent partout n'attire plus rien.
- **Fond** : fond de page principal. Une variation autorisée (teinte du même HEX éclaircie) pour
  les sections alternées, calculée à partir du fond fourni — pas une nouvelle couleur inventée.
- **Texte** : texte courant. Les nuances de gris intermédiaires se calculent en opacité/`color-mix`
  à partir du texte et du fond fournis, jamais comme nouveaux codes hexadécimaux arbitraires.

Chaque HEX fourni est utilisé **exactement**, sans réglage esthétique. Si un contraste est
insuffisant (§ 3.4), on corrige la mise en page ou la couleur de texte/fond à l'intérieur de la
palette, on ne repeint pas la marque.

### 3.2 Typographies

Typographies fournies : `à demander au client — à défaut, pile système (voir la consigne du gabarit sur les polices non fournies)`.

- Charger **au maximum 2 familles** (titres + texte) et **au maximum 4 graisses** au total.
- **Auto-hébergement obligatoire** des fichiers `.woff2` dans `public/fonts/` (voir § 5.4).
  Si l'auto-hébergement est impossible, utiliser Google Fonts avec `display=swap`, `preconnect`
  et `preload` de la police utilisée dans le premier écran.
- Tailles hiérarchisées et fluides : `clamp()` pour les titres (par ex. de `1.75rem` à `3rem`
  entre 360 px et 1440 px de large). Interligne 1.5–1.7 pour le corps de texte.
- Longueur de ligne du corps limitée à **60–75 caractères**.
- Si `à demander au client — à défaut, pile système (voir la consigne du gabarit sur les polices non fournies)` est incomplet, utiliser une pile système (`system-ui, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif`) — **et pas** une police fantaisie inventée.

### 3.3 Logo

- Utiliser **uniquement** le fichier logo présent dans `assets/`. Ne pas redessiner, ne pas
  recolorer, ne pas déformer, ne pas ajouter d'effet (ombre, dégradé, contour).
- Prévoir un espace de respiration d'au moins la hauteur du « N » du nom autour du logo.
- Variante claire/foncée : si aucune variante n'est fournie, poser le logo sur le fond fourni
  (ou sur un aplat de la palette offrant un contraste suffisant) et ne pas fabricer de variante.
- Si aucun logo n'est fourni : composer un **logotype typographique** avec le nom exact
  `La Tradition` dans la police de titre de à demander au client — à défaut, pile système (voir la consigne du gabarit sur les polices non fournies) — c'est la seule création
  graphique autorisée, et elle doit rester littérale (le nom, rien d'autre).
- `alt` du logo : « La Tradition — Boulangerie-pâtisserie à Houplines ».

### 3.4 Contrastes (contrôle obligatoire avant livraison)

- Texte courant : **≥ 4.5:1** avec son fond.
- Grand texte (≥ 24 px, ou ≥ 19 px en gras) : **≥ 3:1**.
- Composants d'interface, bordures de champs, icônes porteuses de sens, indicateur de focus :
  **≥ 3:1**.
- Le texte ne porte jamais seul une information (un lien dans un paragraphe est souligné ou
  distingué autrement que par la couleur).
- Le texte reste lisible sur les photos : voile uni, aplat ou bandeau aux couleurs de la palette,
  contrôlé et non approximatif.
- Vérifier chaque paire (fond/texte, bouton/texte du bouton, survol, page d'erreur 404) avec un
  outil de calcul de contraste et **consigner les ratios obtenus** dans `docs/RECETTE.md`.

### 3.5 Interdits visuels

- Aucune couleur hors palette (pas de « je trouve que ça manque de vert »).
- Aucun dégradé, ombre portée lourde, effet de verre, néon ou animation spectaculaire non prévus
  par l'identité fournie.
- Aucune banque d'images génériques laissant croire à une photo du commerce ; seules les photos
  de `NON FOURNI — à demander au client (10 à 20 photos : devanture, comptoir, produits, équipe)` / `assets/` sont utilisables. Une illustration abstraite neutre est
  tolérée pour un fond, jamais pour représenter un produit ou une personne du commerce.
- Aucun carrousel automatique, aucune fenêtre modale d'accueil, aucun son automatique.
- Aucun texte écrit dans une image (sauf le logo), ni information critique uniquement visuelle.
- Aucun framework CSS ou composant dont le style écrase l'identité (pas de thème clé en main
  appliqué par-dessus la palette).

---

## 4. Contenu et arborescence

Le contenu est **rédigé par l'intégrateur contenu** (§ 9), à partir des seuls faits fournis.
Langue : **français**, ton : **chaleureux, artisanal, sobre**.

### 4.1 Arborescence

```
/                            Accueil
/prestations                 Prestations / services
/a-propos                    À propos
/galerie                     Galerie photos
/avis                        Avis clients (uniquement si NON FOURNI — aucun avis vérifiable collecté ; la page « Avis » ne doit pas être publiée tant que le client n'a pas fourni ses vrais avis est fourni)
/horaires-acces              Horaires et accès (carte + itinéraire)
/contact                     Contact (formulaire + coordonnées)
/mentions-legales            Mentions légales
/politique-de-confidentialite Politique de confidentialité
/404                          Page d'erreur
```

- Une seule URL par contenu, en minuscules, avec tirets, sans accent, sans paramètre, sans
  extension `.html` dans les liens internes.
- Chaque page a **un seul `h1`**, une hiérarchie de titres continue (pas de saut h1→h3),
  un fil d'Ariane (sauf accueil), un titre de document unique (§ 6.1) et une action de contact
  visible (téléphone cliquable `tel:` et lien vers `/contact`).
- `/avis` n'existe que si des avis réels sont fournis. S'ils le sont, chaque avis affiche le
  verbatim exact, le prénom/nom ou pseudonyme tel que fourni, la source (Google, Facebook,
  Instagram…) et la date — sans retouche, sans réécriture, sans traduction.

### 4.2 Accueil (`/`)

Sections attendues, dans cet ordre :

1. **En-tête** : logo, nom, navigation (Prestations, À propos, Galerie, Horaires et accès, Contact),
   téléphone cliquable, lien avis si la page existe. Menu utilisable au clavier, fermeture par
   `Échap`, focus piégé quand le menu mobile est ouvert.
2. **Héros** : titre `h1` explicite (`La Tradition` + `Boulangerie-pâtisserie` + `Houplines`), une phrase
   de positionnement tirée de `boulangerie-pâtisserie artisanale de proximité à Houplines (Nord), clientèle de quartier et de passage`, deux actions (Appeler — destination
   `+33320541068` ; « Nous contacter » → `/contact`), rappel des horaires du jour, photo de
   `NON FOURNI — à demander au client (10 à 20 photos : devanture, comptoir, produits, équipe)` ou aplat aux couleurs de la palette.
3. **Bandeau de confiance** : 3 à 4 atouts issus **strictement** de `à compléter par le client (3 à 4 atouts vérifiables : fabrication sur place, horaires larges, spécialité locale, commandes de fêtes)`, en 5–8 mots chacun.
   Si `à compléter par le client (3 à 4 atouts vérifiables : fabrication sur place, horaires larges, spécialité locale, commandes de fêtes)` fournit moins de 3 items, afficher ceux disponibles et ne rien compléter.
4. **Prestations clés** : les 3 à 6 prestations principales issues de `pain et viennoiseries au quotidien ; pâtisseries ; commandes de fêtes (Noël, Pâques, communions) ; snacking le midi ; dépôt de pain si applicable — liste à confirmer par le client`, chacune avec
   son intitulé exact, une description de 2–3 phrases construits sur les faits, et un lien vers
   `/prestations#<ancre>`.
5. **À propos en bref** : 60–100 mots à partir de `boulangerie-pâtisserie artisanale de proximité à Houplines (Nord), clientèle de quartier et de passage` et `à compléter par le client (3 à 4 atouts vérifiables : fabrication sur place, horaires larges, spécialité locale, commandes de fêtes)`, plus un lien
   vers `/a-propos`.
6. **Aperçu galerie** : 3 à 6 photos de `NON FOURNI — à demander au client (10 à 20 photos : devanture, comptoir, produits, équipe)` avec `alt` descriptif, lien vers `/galerie`.
   Absent si aucune photo fournie.
7. **Avis** : uniquement si `NON FOURNI — aucun avis vérifiable collecté ; la page « Avis » ne doit pas être publiée tant que le client n'a pas fourni ses vrais avis` est fourni — 2 à 3 avis les plus représentatifs,
   verbatim + source, lien vers `/avis`. Sinon, **omettre la section**.
8. **Horaires et accès en bref** : horaires de `à demander au client`, adresse `à vérifier, 59116 Houplines`, lien
   « Itinéraire » vers une carte, lien vers `/horaires-acces`.
9. **Appel à l'action final** : une phrase, le téléphone, le formulaire.
10. **Pied de page** : nom, activité, adresse complète, téléphone, e-mail, horaires résumés, réseaux
    fournis (`aucun compte connu`, `aucune page connue`), liens mentions légales et politique de confidentialité,
    année courante. Le bloc adresse+horaires du pied de page est identique sur toutes les pages.

Poids visé : 400–700 mots. Le visiteur doit pouvoir appeler sans faire défiler plus d'un écran.

### 4.3 Prestations (`/prestations`)

- `h1` : « Nos prestations » ou formulation dérivée de `Boulangerie-pâtisserie`.
- **Une section par prestation** listée dans `pain et viennoiseries au quotidien ; pâtisseries ; commandes de fêtes (Noël, Pâques, communions) ; snacking le midi ; dépôt de pain si applicable — liste à confirmer par le client`, dans l'ordre fourni :
  - un `h2` reprenant l'intitulé exact de la prestation (ancre stable, ex. `#nom-de-la-prestation`) ;
  - une description de 3–6 phrases construites sur les faits disponibles ;
  - la liste des inclusions (« ce qui est compris ») si `pain et viennoiseries au quotidien ; pâtisseries ; commandes de fêtes (Noël, Pâques, communions) ; snacking le midi ; dépôt de pain si applicable — liste à confirmer par le client` la fournit ;
  - les modalités utiles **si fournies** (durée, sur rendez-vous, délai, déplacement, zone desservie) ;
  - un appel à l'action (appel ou devis via `/contact`).
- Aucun prix, aucun tarif indicatif, aucune durée inventée. Si `pain et viennoiseries au quotidien ; pâtisseries ; commandes de fêtes (Noël, Pâques, communions) ; snacking le midi ; dépôt de pain si applicable — liste à confirmer par le client` liste un prix, l'afficher
  exactement (avec le libellé fourni, ex. « à partir de », sans le transformer).
- Terminer par une accroche de contact et un rappel de la zone desservie (`Houplines`).
- Pas de FAQ inventée : une FAQ n'est autorisée que si les questions-réponses proviennent de faits
  fournis (les reprendre sans les reformuler en promesse).

### 4.4 À propos (`/a-propos`)

- Reprise du parcours, de l'histoire, de l'équipe — **uniquement** ce qui est fourni.
- 150–400 mots, ton chaleureux, artisanal, sobre, phrases courtes, aucune formule creuse.
- Si l'information disponible se limite à `boulangerie-pâtisserie artisanale de proximité à Houplines (Nord), clientèle de quartier et de passage` et `à compléter par le client (3 à 4 atouts vérifiables : fabrication sur place, horaires larges, spécialité locale, commandes de fêtes)`, écrire une page
  courte et honnête (100–150 mots) ; ne pas gonfler avec de la biographie inventée.
- Photo de l'équipe/du lieu si fournie dans `NON FOURNI — à demander au client (10 à 20 photos : devanture, comptoir, produits, équipe)`, `alt` factuel.

### 4.5 Galerie (`/galerie`)

- Uniquement les photos de `NON FOURNI — à demander au client (10 à 20 photos : devanture, comptoir, produits, équipe)` / `assets/`. Aucune image de banque d'images.
- Grille responsive mobile-first, légende factuelle si fournie, `alt` décrivant ce que montre
  l'image (pas de « photo » ni de nom de fichier).
- Images en WebP/AVIF, `width`/`height` explicites, `loading="lazy"` sauf les deux premières,
  clic → version agrandie (lightbox accessible : focus géré, `Échap`, pas de dépendance payante).
- Page omise si aucune photo n'est fournie.

### 4.6 Avis (`/avis`)

- Créée **si et seulement si** `NON FOURNI — aucun avis vérifiable collecté ; la page « Avis » ne doit pas être publiée tant que le client n'a pas fourni ses vrais avis` contient des avis.
- Chaque avis : verbatim exact, auteur tel que fourni, source, date, et lien vers la source si
  une URL est fournie.
- Interdiction de : reformuler, corriger l'orthographe, traduire, compléter une phrase, inventer
  une note moyenne, afficher un logo « Google » ou « Facebook » détourné, afficher des étoiles
  qui ne correspondent pas à une note réellement fournie.
- Pas de témoignage anonyme transformé en client nommé.

### 4.7 Horaires et accès (`/horaires-acces`)

- Tableau des horaires **exactement** comme dans `à demander au client` (jours, plages, fermetures,
  jours fériés si fournis), en texte HTML (pas dans une image, pas dans un canvas).
- Adresse complète : `à vérifier, 59116 Houplines`, `59116` si fourni, `Houplines`.
- Carte intégrée : **iframe OpenStreetMap** (gratuit, sans consentement requis) centrée sur
  l'adresse ; si `50.6943663, 2.9123959` est fourni, l'utiliser, sinon géocoder l'adresse une fois et
  consigner les coordonnées dans un ADR. Prévoir un lien « Itinéraire » ouvrant Google Maps ou
  OSM dans un nouvel onglet, et un lien `tel:+33320541068`.
- Si des informations de stationnement / transports / accès PMR sont fournies, une section les
  reprend ; sinon, ne rien écrire sur le sujet.
- Ajouter un bloc « aujourd'hui : ouvert/fermé de X à Y » calculé côté client en JavaScript
  uniquement **à partir de `à demander au client`** ; si les horaires sont irréguliers au point de rendre
  le calcul ambigu, afficher simplement le jour courant et ses plages.

### 4.8 Contact (`/contact`)

- Coordonnées complètes et cliquables (`tel:`, `mailto:`), horaires, adresse, réseaux fournis.
- Formulaire : nom, e-mail ou téléphone (au moins un des deux), message, case de consentement
  RGPD **non pré-cochée** (§ 8.3). Champs obligatoires balisés, erreurs annoncées en texte.
- Le formulaire fonctionne **sans backend** : Netlify Forms si l'hébergement est Netlify, sinon
  `mailto:` correctement encodé (`action="mailto:à demander au client (aucun email public trouvé)"` + `enctype="text/plain"`) — dans ce
  cas afficher aussi `à demander au client (aucun email public trouvé)` et `+33320541068` en évidence, un `mailto:` échouant souvent
  sur mobile.
- Message de confirmation après envoi (page de remerciement ou état succès), sans promesse de
  délai de réponse non fournie.
- Si le formulaire n'est pas fonctionnel à la livraison, il est **désactivé** et remplacé par les
  coordonnées — un formulaire qui ne délivre rien est un défaut bloquant.

### 4.9 Mentions légales (`/mentions-legales`)

Éditeur (nom, forme/statut si fourni, adresse, téléphone, e-mail), `à demander au client (mentions légales)` s'il est fourni,
responsable de la publication `Moh Amed (prospection vitrines)`, hébergeur `à préciser au moment du déploiement (mentions légales + politique de confidentialité)`, propriété
intellectuelle (textes, photos, logo), et clause de responsabilité pour les liens externes.
Toute valeur inconnue reste en commentaire `TODO_CONTENU` + ligne dans `docs/A-COMPLETES.md`.

### 4.10 Politique de confidentialité (`/politique-de-confidentialite`)

Nature des données collectées (le formulaire uniquement), finalité (répondre à la demande),
base légale (intérêt légitime / consentement), destinataire (le commerçant, et le cas échéant
le service de formulaire `à préciser au moment du déploiement (mentions légales + politique de confidentialité)`), durée de conservation `à préciser avec le client (par défaut : 12 mois pour les messages du formulaire)`,
droits (accès, rectification, effacement, opposition, réclamation CNIL) avec l'e-mail de contact
`à demander au client (aucun email public trouvé)`, absence de transfert hors UE non justifié, et mention explicite de l'absence de
cookie de mesure d'audience ou publicitaire (§ 8.2).

### 4.11 Page 404

Message clair, ton chaleureux, artisanal, sobre, liens vers l'accueil, les prestations et le contact. Pas de texte
humoristique long.

### 4.12 Règles de rédaction

- Phrases courtes, voix active, vocabulaire du métier et de la clientèle locale.
- Dire la ville (`Houplines`) au moins une fois dans le `h1` ou le premier paragraphe de l'accueil,
  de `/prestations` et de `/horaires-acces`.
- Aucune tournure creuse (« solutions innovantes », « au service de l'excellence »), aucun
  superlatif non démontré (« le meilleur », « n°1 »).
- Aucune répétition mécanique de mots-clés ; `boulangerie Houplines, pâtisserie Houplines, pain Houplines, commande pâtisserie Houplines, boulangerie ouverte dimanche Houplines` s'intègre naturellement dans le
  corps de texte aux endroits où c'est vrai.
- Les données factuelles (adresse, téléphone, horaires) sont **identiques au caractère près** sur
  toutes les pages, dans le JSON-LD et dans les balises meta (§ 6.5).

---

## 5. Stack technique imposée

### 5.1 Cœur du projet

**Option A — recommandée.** Astro + Tailwind CSS, génération **statique** (`output: 'static'`).

- Astro produit du HTML sans framework client lourd, ce qui sert directement le budget de
  performance (§ 7) tout en offrant des composants réutilisables (en-tête, pied de page, cartes,
  formulaire) et la génération du sitemap.
- Version : dernière stable d'Astro au moment du build (`npm create astro@latest`). Tailwind ajouté
  via `npx astro add tailwind`.
- JavaScript côté client **uniquement** pour : menu mobile, lightbox de galerie, calcul du statut
  « ouvert/fermé ». Pas de framework réactif client (React/Vue/Svelte) pour un site vitrine.

**Option B — fallback explicite.** Si le client ne veut **aucune étape de build** ni `node_modules`
(un dépôt de fichiers déposables par FTP), écrire du **HTML/CSS/JS vanilla** :

- `index.html`, `prestations.html`, `a-propos.html`, `galerie.html`, `avis.html`,
  `horaires-acces.html`, `contact.html`, `mentions-legales.html`,
  `politique-de-confidentialite.html`, `404.html`, `styles.css`, `script.js` (seul fichier JS,
  non bloquant, avec `defer`), `sitemap.xml`, `robots.txt`.
- Convention de test local : `python3 -m http.server 8080` (ou `npx serve`).
- Le fallback ne change rien au reste de ce cahier des charges : contenu, SEO, accessibilité,
  performance, conformité et recette sont identiques.

Choisir **une seule** option, la consigner dans un ADR, et ne pas mélanger les deux.

### 5.2 Dépendances

- Uniquement des dépendances **gratuites et auto-hébergeables**, avec licence compatible usage
  commercial (MIT, Apache-2.0, ISC, BSD).
- Interdits : services payants, API nécessitant une clé, polices payantes, widgets imposant un
  abonnement, plateformes « site builder » propriétaires, scripts d'analytics payants.
- Interdits : scripts de suivi tiers (Google Analytics, Meta Pixel, Hotjar, GTM, reCAPTCHA v3,
  polices chargées depuis un CDN bloquant). Aucun cookie non technique (§ 8.2).
- Chaque dépendance ajoutée figure dans un ADR avec sa justification ; zéro dépendance par défaut.
- Si Astro est utilisé : ajouter `@astrojs/sitemap` (gratuit) pour `sitemap.xml`.

### 5.3 Formulaire de contact sans backend

Un site statique n'exécute pas de serveur. Deux implémentations acceptées :

1. **Netlify Forms** (`<form name="contact" method="POST" data-netlify="true">` + champ caché
   `form-name`, page de succès dédiée). Aucun script tiers, aucune clé.
2. **`mailto:`** (`action="mailto:à demander au client (aucun email public trouvé)" method="POST" enctype="text/plain"`) — solution de
   repli, à accompagner visiblement du téléphone et de l'e-mail.

Dans les deux cas : champ honeypot anti-spam (`<input type="text" name="_trap" hidden>`), validation
côté client en HTML natif (`required`, `type="email"`, `maxlength`), et aucune donnée envoyée à un
tiers autre que l'hébergeur/le service de formulaire déclaré dans la politique de confidentialité.

### 5.4 Polices

- **Auto-hébergées** par défaut : fichiers `.woff2` (et `.woff` si nécessaire) dans `public/fonts/`,
  déclaration `@font-face` avec `font-display: swap` et `unicode-range` si sous-ensemble.
- Si l'auto-hébergement est impossible : Google Fonts **limité** aux familles de `à demander au client — à défaut, pile système (voir la consigne du gabarit sur les polices non fournies)`,
  avec `<link rel="preconnect">` vers `fonts.gstatic.com` et `<link rel="preload">` sur la police
  du premier écran. Documents et justifie le choix dans un ADR.
- Ne pas dépasser 4 fichiers de police au total et ~120 Ko transférés pour les polices.

### 5.5 Images et médias

- Formats : **AVIF puis WebP** en `<picture>`/`srcset`, `<img>` avec **`width` et `height`
  explicites** (aucun décalage de mise en page), `loading="lazy"` sauf visuel du premier écran
  (`loading="eager"` + `fetchpriority="high"`), `decoding="async"`.
- Générer systématiquement des variantes de largeur : 480, 768, 1024, 1600 px, plus une version
  `2x` pour le visuel du héros. Jamais d'image servie plus large que son conteneur.
- Poids cible : héros ≤ 200 Ko, image de contenu ≤ 120 Ko. Compression agressive mais sans
  artefacts visibles sur les visages et les produits.
- Toute image porte un `alt` utile et factuel ; `alt=""` uniquement pour une image purement
  décorative.
- Vidéos : si une vidéo est fournie, `<video>` sans lecture automatique, `preload="metadata"`,
  avec sous-titres si elle contient de la parole ; sinon poster image + lien externe.

### 5.6 Structure de fichiers attendue (option A)

```
/
├── AGENTS.md
├── README.md
├── astro.config.mjs
├── package.json
├── public/
│   ├── robots.txt
│   ├── favicon.svg
│   ├── fonts/
│   └── images/            (AVIF + WebP + fallbacks)
├── src/
│   ├── layouts/Base.astro
│   ├── components/        Header, Footer, Hero, Carte, FormulaireContact,
│   │                      FilAriane, Horaires, CarteAcces, Seo, JsonLdLocalBusiness
│   ├── content/           prestations.md, avis.json  (données issues des variables)
│   ├── pages/             index, prestations, a-propos, galerie, avis,
│   │                      horaires-acces, contact, mentions-legales,
│   │                      politique-de-confidentialite, 404
│   └── styles/global.css  (variables de palette + utilitaires)
└── docs/
    ├── decisions/         ADR-0001-….md
    ├── A-COMPLETES.md
    └── RECETTE.md
```

- Les données factuelles (NAP, horaires, réseaux, palette) sont définies **une seule fois** dans
  un fichier de configuration (`src/config/site.ts` ou `src/content/site.json`) et importées par
  les composants. Toute duplication de l'adresse ou du téléphone dans les templates est un défaut
  à corriger (§ 6.5).
- `sitemap.xml` généré à la compilation (ou écrit à la main pour l'option B), `robots.txt` explicite.

### 5.7 Déploiement

Le site doit se déployer par un simple glisser-déposer du dossier `dist/` (ou des fichiers
statiques) chez l'hébergeur, sans base de données, sans cron, sans variable d'environnement
obligatoire. Documenter dans `README.md` : la commande de build, le dossier à publier, la
configuration du domaine `à définir avec le client (proposition : la-tradition-houplines.fr)`, l'activation du HTTPS, et la procédure pour que
le client modifie lui-même un texte, un horaire ou une photo.

### 5.8 Commandes

```bash
# Option A — Astro
npm create astro@latest . -- --template minimal --no-install --no-git --typescript strict
npm install
npx astro add tailwind sitemap
npm run dev        # http://localhost:4321
npx astro check    # vérification types/templates, doit retourner 0 erreur
npm run build      # génère dist/
npm run preview    # http://localhost:4321 (sert dist/)

# Option B — HTML/CSS/JS vanilla
python3 -m http.server 8080     # http://localhost:8080
# ou : npx serve -l 8080 .
```

Ne pas ajouter d'étape de build supplémentaire (pas de Webpack/Vite custom, pas de transpileur,
pas de générateur de CSS par-dessus Tailwind).

---

## 6. SEO local

L'objectif est d'être visible sur les requêtes locales autour de Houplines liées à Boulangerie-pâtisserie et
de se présenter de façon identique sur le web. Voir `boulangerie Houplines, pâtisserie Houplines, pain Houplines, commande pâtisserie Houplines, boulangerie ouverte dimanche Houplines` pour la liste de travail.

### 6.1 Titres et meta descriptions (par page)

- `<title>` : **50–60 caractères**, unique par page, contenant le mot-clé local principal et
  La Tradition. Modèle : « Boulangerie-pâtisserie à Houplines — La Tradition » pour l'accueil, puis
  « <prestation> à Houplines — La Tradition » pour les pages internes.
- `<meta name="description">` : **140–160 caractères**, unique, factuelle, contenant Houplines et
  un appel à l'action. Aucune description dupliquée entre pages.
- `<link rel="canonical">` absolue en `à définir avec le client (proposition : la-tradition-houplines.fr)` sur chaque page.
- Attribut `lang` sur `<html>` : code de langue repris de français, sans la région s'il en comporte
  une (`fr-FR` → `fr`, `nl-BE` → `nl`), plus `<meta charset="utf-8">` et
  `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- Balises Open Graph et Twitter Card : `og:title`, `og:description`, `og:type` (`website` /
  `business.business`), `og:url`, `og:image` (1200×630 issue d'une photo fournie ou d'un aplat
  avec le logotype), `og:site_name`, `og:locale`.
- Favicon : `favicon.svg` (ou `.ico`) réel, pas de 404 sur `/favicon.ico`.
- `<link rel="preload">` pour la police et le visuel du premier écran.

### 6.2 JSON-LD `LocalBusiness`

À insérer sur **toutes les pages** (composant `JsonLdLocalBusiness`), avec des valeurs strictement
conformes aux variables. `@type` = `Bakery` si fourni et pertinent, sinon
`LocalBusiness` (ou un sous-type exact correspondant à `Boulangerie-pâtisserie` : `Restaurant`,
`Bakery`, `BeautySalon`, `AutoRepair`, etc.).

```json
{
  "@context": "https://schema.org",
  "@type": "Bakery",
  "@id": "à définir avec le client (proposition : la-tradition-houplines.fr)/#commerce",
  "name": "La Tradition",
  "description": "<positionnement, une phrase, issue de boulangerie-pâtisserie artisanale de proximité à Houplines (Nord), clientèle de quartier et de passage>",
  "url": "à définir avec le client (proposition : la-tradition-houplines.fr)",
  "image": "à définir avec le client (proposition : la-tradition-houplines.fr)/images/<visuel-principal>",
  "telephone": "+33320541068",
  "email": "à demander au client (aucun email public trouvé)",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "à vérifier, 59116 Houplines",
    "addressLocality": "Houplines",
    "postalCode": "59116",
    "addressCountry": "FR"
  },
  "geo": { "@type": "GeoCoordinates", "latitude": "<latitude issue de 50.6943663, 2.9123959>", "longitude": "<longitude issue de 50.6943663, 2.9123959>" },
  "areaServed": [{ "@type": "City", "name": "Houplines" }],
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday", "Tuesday"],
      "opens": "09:00",
      "closes": "18:30"
    }
  ],
  "sameAs": ["aucun compte connu", "aucune page connue"],
  "priceRange": "<gamme de prix, uniquement si elle est fournie>"
}
```

Règles :

- **Construire `openingHoursSpecification` à partir de `à demander au client`**, en listant chaque jour
  séparément ; un jour fermé n'apparaît pas. Si un horaire est marqué « sur rendez-vous » ou
  irrégulier, l'exclure du JSON-LD plutôt que produire une donnée fausse.
- `geo` : uniquement si `50.6943663, 2.9123959` est fourni (ou géocodage documenté dans un ADR). Sinon **omettre**
  la clé `geo`.
- `priceRange` : uniquement si une information de gamme de prix est fournie. Sinon **omettre**.
- `sameAs` ne contient que les profils réellement fournis (retirer toute valeur vide).
- Le JSON-LD doit être validé (parse JSON + test du Rich Results Test de Google) et **cohérent au
  caractère près** avec le texte visible de la page.
- Selon le contenu, ajouter `BreadcrumbList` (fil d'Ariane) et `WebSite` (avec `url` et `name`).
  Ne pas ajouter de `FAQPage` ni de `AggregateRating` sans contenu réel correspondant (§ 0.3).

### 6.3 `sitemap.xml` et `robots.txt`

- `sitemap.xml` : **une seule** URL (pas de `sitemap_index.xml` pour 10 pages), URLs absolues en
  `à définir avec le client (proposition : la-tradition-houplines.fr)`, `<lastmod>` au format ISO 8601 (date de build), `<changefreq>` et
  `<priority>` sobres. Exclure `404`.
- `robots.txt` :

  ```
  User-agent: *
  Allow: /
  Sitemap: à définir avec le client (proposition : la-tradition-houplines.fr)/sitemap.xml
  ```

- `sitemap.xml` référencé aussi depuis `robots.txt` et depuis chaque page si la balise est utile ;
  `<link rel="sitemap">` non nécessaire.
- Les pages légales peuvent être indexées (aucun `noindex` sauf besoin explicite).

### 6.4 Structure et fil d'Ariane

- Fil d'Ariane visuel **et** balisé (`<nav aria-label="Fil d'Ariane">` + `BreadcrumbList` JSON-LD)
  sur toutes les pages sauf l'accueil.
- URL courtes et lisibles, identiques aux intitulés de navigation.
- Hiérarchie HTML significative : `header`, `nav`, `main`, `section` avec `aria-labelledby`,
  `footer`. Titres descriptifs, pas « Section 1 ».
- Texte d'ancre explicite (« voir nos prestations à Houplines »), jamais « cliquer ici ».
- Contenu visible immédiatement : pas d'affichage conditionnel en JavaScript pour le corps de texte
  (les robots n'attendent pas). Le JavaScript ne sert qu'aux interactions (§ 5.1).

### 6.5 Cohérence NAP et Google Business

- Le nom, l'adresse et le téléphone (`La Tradition`, `à vérifier, 59116 Houplines`, `+33320541068`) sont
  **strictement identiques** dans : le pied de page, la page contact, la page horaires et accès,
  les mentions légales, le JSON-LD et les balises meta. Vérification automatisée exigée (§ 10.2).
- Si `à vérifier — aucune fiche revendiquée identifiée au 2026-09-29` est fourni, comparer les valeurs de la fiche Google Business aux
  variables ; **toute divergence** est signalée dans `docs/A-COMPLETES.md` (c'est au commercial de
  la trancher, pas à l'IA de choisir une version).
- `sameAs` du JSON-LD reprend les réseaux fournis ; les liens de réseaux s'ouvrent en nouvel onglet
  avec `rel="noopener"`.
- Ajouter un lien discret vers la fiche Google Business (avis/itinéraire) si l'URL est fournie.
- Mots-clés : utiliser `boulangerie Houplines, pâtisserie Houplines, pain Houplines, commande pâtisserie Houplines, boulangerie ouverte dimanche Houplines` en respectant la densité naturelle — un mot-clé
  principal par page, ses variantes dans les `h2`, et les noms de communes **uniquement** s'ils
  font partie des faits fournis. Aucun bourrage, aucune liste de villes inventée, aucune page
  « SEO » vide créée pour capter du trafic.

---

## 7. Accessibilité et performance

### 7.1 Accessibilité — cible WCAG 2.2 niveau AA

Exigences vérifiables : la grille de recette fournie avec ce prompt (section D, A11Y). Points non
négociables :

- Structure sémantique valide, un seul `h1` par page, hiérarchie de titres continue.
- Navigation **complète au clavier** : ordre de tabulation logique, lien d'évitement (« Aller au
  contenu ») — et `:focus-visible` toujours visible, contraste ≥ 3:1, jamais supprimé.
- Cibles tactiles ≥ 24×24 px CSS (recommandé 44×44), espacement suffisant entre liens (`2.5.8`).
- Contrastes conformes (§ 3.4), vérifiés page par page.
- Images : `alt` utile et factuel, `alt=""` pour le décoratif, pas de texte essentiel en image.
- Formulaires : chaque champ a un `<label>` associé, les erreurs sont annoncées en texte (pas
  seulement en rouge), `autocomplete` renseigné, `aria-describedby` sur les aides.
- Menu mobile : `aria-expanded`, fermeture `Échap`, focus piégé et rendu à l'élément déclencheur.
- Lightbox galerie : navigation clavier, `Échap`, focus géré, pas de piège de focus.
- Couleurs : aucune information portée uniquement par la couleur.
- Animations respectant `prefers-reduced-motion` ; aucun clignotement ; pas de contenu qui bouge
  tout seul.
- Langue déclarée (`lang="français"`), libellés de liens explicites, titres de pages uniques.
- La carte iframe a un `title` décrivant son contenu.
- Textes à 200 % de zoom : pas de perte de contenu, pas de défilement horizontal.

### 7.2 Performance — budgets à tenir

| Mesure | Seuil | Comment |
|---|---|---|
| Lighthouse Performance (mobile) | ≥ 95 | `npx lighthouse --form-factor=mobile --throttling-method=simulate` ou LHCI |
| Lighthouse Accessibilité | ≥ 95 | idem |
| Lighthouse Bonnes pratiques | ≥ 95 | idem |
| Lighthouse SEO | ≥ 95 | idem |
| Poids total transféré page d'accueil | **< 500 Ko** | Somme HTML + CSS + JS + polices + images du premier chargement, mesurée dans l'onglet Network (cache vide) |
| Poids total transféré pages internes | < 400 Ko | idem |
| LCP (mobile, 4G simulée) | < 2,5 s | Lighthouse / PageSpeed Insights |
| CLS | < 0,1 | `width`/`height` explicites, polices auto-hébergées |
| INP | < 200 ms | JavaScript minimal, tâches longues interdites |
| Requêtes HTTP page d'accueil | ≤ 25 | Fusion, `srcset` raisonné, pas de dépendance inutile |
| CSS bloquant | < 50 Ko | Tailwind purgé |
| JS exécuté page d'accueil | < 30 Ko | Astro sans framework client |

- Aucun script tiers, aucune police externe bloquante, aucune image non compressée.
- Vérifier les quatre axes **et** les budgets de poids sur la version **`npm run build` + `preview`**,
  pas sur le serveur de développement.
- Consigner les scores dans `docs/RECETTE.md` (page d'accueil **et** une page interne).

---

## 8. Conformité

### 8.1 Mentions légales

Page obligatoire et accessible depuis le pied de page de toutes les pages : identité de l'éditeur,
adresse, téléphone, e-mail, `à demander au client (mentions légales)` s'il est fourni, responsable de la publication, hébergeur
(nom, adresse, téléphone), mention de propriété intellectuelle. Aucune valeur factice : une
information inconnue reste en `TODO_CONTENU` dans le code **et** listée dans `docs/A-COMPLETES.md`.

### 8.2 RGPD et cookies

- **Aucun cookie ni traceur par défaut** : pas d'analytics, pas de pixel, pas de vidéo YouTube
  intégrée, pas de bouton social qui trace, pas de reCAPTCHA. Carte = OpenStreetMap (§ 4.7).
- Conséquence attendue : **aucune bannière de consentement n'est affichée**, et s'il n'y a aucun
  traceur, l'absence de bannière est correcte — le documenter dans un ADR.
- Si le client exige plus tard une mesure d'audience, l'alternative acceptée est une solution
  auto-hébergée et sans cookie ou respectant `Do Not Track`, chargée **après** consentement.
  Ne pas l'implémenter de sa propre initiative.
- Aucun envoi de données à un tiers non déclaré. Le service de formulaire (`à préciser au moment du déploiement (mentions légales + politique de confidentialité)`) est
  nommé dans la politique de confidentialité.
- Aucun stockage local superflu (`localStorage` uniquement si nécessaire au fonctionnement, et
  documenté).

### 8.3 Formulaire et données personnelles

- Case de consentement explicite, **non pré-cochée**, avec lien vers `/politique-de-confidentialite`.
- Aucune donnée collectée au-delà du nécessaire (nom, moyen de contact, message).
- Durée de conservation annoncée : `à préciser avec le client (par défaut : 12 mois pour les messages du formulaire)` (si `à compléter par le client`,
  écrire la mention générique « le temps nécessaire au traitement de la demande » et pointer le
  TODO dans `docs/A-COMPLETES.md`).
- Si Netlify Forms est utilisé : mentionner clairement dans la politique de confidentialité que les
  messages transitent par ce service, en tant que sous-traitant.
- Pas de capture d'adresse IP à des fins de mesure, pas de profilage.

### 8.4 Accessibilité réglementaire

- La cible WCAG 2.2 AA (§ 7.1) doit être atteinte et consignée dans `docs/RECETTE.md`.
- Ajouter dans la politique de confidentialité ou les mentions légales une phrase de contact pour
  signaler un problème d'accessibilité (`à demander au client (aucun email public trouvé)`), sauf si le client s'y oppose explicitement
  (alors : rien, pas d'invention).
- Ne pas affirmer une conformité RGAA/WCAG « certifiée » : écrire « conçu pour respecter » et
  laisser la vérification humaine.

---

## 9. Organisation des agents

Le travail se fait en **6 rôles**. Un même agent peut tenir plusieurs rôles, mais **aucun rôle ne
s'auto-valide** : le testeur QA et le relecteur sont distincts de celui qui a produit le code ou
le texte.

| Rôle | Responsabilité | Artefacts attendus |
|---|---|---|
| **PO / chef de projet** | Cadrer, découper en lots, tenir `docs/A-COMPLETES.md`, arbitrer les ADR, vérifier que le périmètre est tenu | `docs/A-COMPLETES.md`, ADR, plan de lots, rapport final de recette |
| **Développeur front** | Structure du projet, composants, styles, palette, responsive, interactions JS, build | `src/` (ou fichiers du fallback), `styles/global.css`, configuration |
| **Intégrateur contenu** | Rédiger tous les textes à partir des faits, peupler les composants, vérifier chaque phrase contre les variables | Contenus de toutes les pages + traçabilité (quelle variable alimente quelle phrase) |
| **Référenceur local** | Titres/meta, JSON-LD, sitemap, robots, fil d'Ariane, cohérence NAP, mots-clés | Balises SEO, fichiers SEO, vérification NAP |
| **Testeur QA** | Exécuter la grille de recette fournie avec ce prompt sur le build, mesurer les budgets | `docs/RECETTE.md` rempli, anomalies listées |
| **Relecteur** | Contrôle indépendant : zéro donnée inventée, zéro faute de sens, conformité aux variables, lisibilité, orthographe, accessibilité | Rapport de relecture avec verdict par critère bloquant |

### Ordre d'intervention

1. **Lot 0 — Cadrage technique** (PO + dev front) : checklist des variables, questions bloquantes,
   choix option A/B, arborescence validée, `AGENTS.md` écrit, squelette du projet et build vide qui passe.
2. **Lot 1 — Accueil** (dev front + intégrateur contenu) : en-tête, héros, sections de l'accueil,
   pied de page, palette et typographies appliquées. Le build doit passer avant le lot suivant.
3. **Lot 2 — Pages internes** (intégrateur contenu + dev front) : prestations, à propos, galerie,
   avis (si `NON FOURNI — aucun avis vérifiable collecté ; la page « Avis » ne doit pas être publiée tant que le client n'a pas fourni ses vrais avis`), horaires et accès, contact, 404.
4. **Lot 3 — SEO et conformité** (référenceur local + intégrateur contenu) : meta, JSON-LD plus
   `sitemap.xml`, `robots.txt`, fil d'Ariane, mentions légales, politique de confidentialité,
   cohérence NAP.
5. **Lot 4 — Qualité** (testeur QA) : Lighthouse, budgets de poids, tests navigateurs (§ 10.2),
   validation HTML, contrastes, clavier, mobile.
6. **Lot 5 — Revue et livraison** (relecteur + PO) : relecture anti-invention, corrections, mise à
   jour de `docs/RECETTE.md`, `README.md`, rapport final.

### Règles de handoff

Chaque passage de relais est un **bloc écrit**, jamais implicite :

```
LOT <n> — <nom>
État : terminé / partiel (détail)
Fichiers touchés : <chemins>
Vérifié comment : <commande exacte + résultat>
Non fait / incertain : <liste>
Risque ouvert : <liste>
```

- Le dev front ne livre pas un lot si `npm run build` échoue ou si `npx astro check` remonte une erreur.
- L'intégrateur contenu ne livre pas un texte dont il ne peut pas nommer la variable source.
- Le référenceur local ne livre pas un JSON-LD non parsable ou divergent du visible.
- Le testeur QA ne valide pas un critère sans avoir la sortie de la commande ou la capture de la
  mesure correspondante.
- Le relecteur n'accepte aucune réponse « c'est probablement bon » : chaque point bloquant exige
  une preuve.

### Critères d'acceptation par lot

- **Lot 0** : projet créé, build vide OK, `AGENTS.md` présent, choix A/B documenté, liste des
  informations manquantes établie.
- **Lot 1** : accueil complet, responsive 360/768/1440 px correct, palette et polices exactes,
  navigation clavier fonctionnelle, contrastes de l'accueil conformes.
- **Lot 2** : toutes les pages de l'arborescence existent (hors `/avis` si aucun avis réel),
  aucun texte sans source, aucune section vide, formulaire fonctionnel ou explicitement désactivé.
- **Lot 3** : `<title>`/description uniques et dans les longueurs, JSON-LD valide, `sitemap.xml`
  et `robots.txt` en place, NAP identique partout, mentions légales et politique de confidentialité
  présentes.
- **Lot 4** : ≥ 95 sur les quatre axes Lighthouse, poids d'accueil < 500 Ko, zéro erreur axe/pa11y
  critique, zéro erreur de validation HTML, tests navigateurs passés.
- **Lot 5** : relecture anti-invention sans anomalie bloquante, `docs/RECETTE.md` complet,
  `README.md` utilisable par le client.

---

## 10. Définition de terminé, recette et commandes

### 10.1 Définition de terminé (tous les points requis)

1. Toutes les pages de § 4.1 existent et sont accessibles depuis la navigation et le pied de page
   (hors `/avis` et `/galerie` si les données correspondantes sont absentes — dans ce cas, aucun
   lien mort ne doit subsister).
2. Aucune donnée inventée : chaque fait traçable à une variable ou à un fichier de `assets/`.
3. Palette et typographies appliquées exactement, contrastes vérifiés.
4. Responsive mobile-first vérifié à 360, 390, 768, 1024 et 1440 px, sans défilement horizontal.
5. SEO : titres/meta uniques, JSON-LD valide, sitemap, robots, fil d'Ariane, NAP identique partout.
6. Accessibilité : WCAG 2.2 AA sur les critères listés, navigation clavier complète, zéro erreur
   critique automatisée.
7. Performance : ≥ 95 sur les quatre axes, budgets de poids tenus.
8. Conformité : mentions légales, politique de confidentialité, zéro traceur, formulaire conforme RGPD.
9. `npm run build` (ou l'équivalent statique) passe sans erreur ni avertissement bloquant ;
   `dist/` se déploie tel quel.
10. `AGENTS.md`, `README.md`, `docs/A-COMPLETES.md`, `docs/decisions/` et `docs/RECETTE.md` présents
    et à jour.
11. Aucun `TODO_CONTENU` restant **visible sur le site** (les TODO vivent en commentaire de code et
    dans `docs/A-COMPLETES.md`).

### 10.2 Checklist de recette (à cocher avec preuve)

Pour chaque point : exécuter, mesurer, écrire le résultat dans `docs/RECETTE.md`. La grille complète,
critère par critère, avec la méthode de vérification, est fournie par le client avec ce prompt
(bloc « Grille de recette » : contenu, SEO, performance, accessibilité, conformité, responsive,
navigateurs, validation technique, livrables).

**Contenu et vérité**

- [ ] Recherche exhaustive des données non fournies : aucun avis, prix, chiffre, certification,
      nom propre inventé (relecture ligne à ligne + comparaison aux variables).
- [ ] Chaque phrase factuelle se rattache à une variable identifiable.
- [ ] Zéro section vide, zéro `lorem ipsum`, zéro « à venir ».
- [ ] NAP identique partout : `grep` du téléphone et de l'adresse dans `dist/`, nombre d'occurrences
      attendu et identique au caractère près.

**SEO**

- [ ] `<title>` et `description` uniques, longueurs respectées.
- [ ] JSON-LD parse valide (extraction + `JSON.parse`) et identique au visible.
- [ ] `sitemap.xml` complet, `robots.txt` valide, canonical absolues.
- [ ] Fil d'Ariane sur toutes les pages internes.

**Performance**

- [ ] Lighthouse ≥ 95 sur les 4 axes, page d'accueil **et** une page interne.
- [ ] Poids d'accueil < 500 Ko, pages internes < 400 Ko.
- [ ] `width`/`height` sur toutes les images, `loading` correct.
- [ ] Zéro script tiers, zéro police externe bloquante.

**Accessibilité**

- [ ] Navigation clavier de bout en bout (menu, formulaire, galerie, lien d'évitement).
- [ ] Focus toujours visible.
- [ ] Contrastes mesurés conformes (AA).
- [ ] `alt` pertinents, formulaire étiqueté, erreurs annoncées en texte.
- [ ] Zoom 200 % sans perte de contenu ni défilement horizontal.

**Conformité**

- [ ] Mentions légales complètes ou TODOs listés dans `docs/A-COMPLETES.md`.
- [ ] Politique de confidentialité présente, cohérente avec le formulaire réellement installé.
- [ ] Zéro cookie déposé au premier chargement (vérifié dans le stockage du navigateur).

**Responsive et navigateurs**

- [ ] 360 / 390 / 768 / 1024 / 1440 px : mise en page, navigation, formulaires, galerie.
- [ ] Derniers Chrome, Firefox, Safari, Edge + Safari iOS et Chrome Android (ou équivalent).
- [ ] Menu mobile, lightbox, statut « ouvert/fermé ».

**Technique**

- [ ] `npm run build` sans erreur, `npx astro check` à 0 erreur.
- [ ] Validation HTML (W3C/nu) sans erreur.
- [ ] Aucun lien cassé (interne et externe), aucun 404 hors page 404 volontaire.
- [ ] HTTPS et redirection du domaine `à définir avec le client (proposition : la-tradition-houplines.fr)` documentés dans `README.md`.

### 10.3 Commandes exactes

```bash
# --- Construction
npm run dev            # développement, http://localhost:4321
npx astro check        # 0 erreur exigée
npm run build          # build statique -> dist/
npm run preview        # vérification sur le build, http://localhost:4321

# --- Recette performance / accessibilité (sur le build servi)
npx lighthouse http://localhost:4321 --form-factor=mobile --output=json --output-path=./docs/lighthouse-accueil.json
npx lighthouse http://localhost:4321/prestations --form-factor=mobile --output=json --output-path=./docs/lighthouse-prestations.json
# ou, en batch :
npx @lhci/cli autorun --collect.staticDistDir=dist --collect.numberOfRuns=1

# --- Accessibilité et qualité du HTML
npx pa11y http://localhost:4321 --standard WCAG2AA
npx html-validate "dist/**/*.html"

# --- SEO / cohérence (adapté à l'hébergeur du projet)
grep -ro "+33320541068" dist/ | wc -l          # occurrence identique au caractère près
grep -ro "à vérifier, 59116 Houplines" dist/ | wc -l
curl -s http://localhost:4321/sitemap.xml | head -20
python3 -c "import json,re,sys; [json.loads(m) for m in re.findall(r'<script type=\"application/ld\\+json\">(.*?)</script>', open('dist/index.html', encoding='utf-8').read(), re.S)]" && echo "JSON-LD OK"

# --- Poids des ressources
du -sh dist/ && du -sh dist/_astro 2>/dev/null
```

Adapter les sélecteurs et noms de fichiers au projet réel ; utiliser la dernière version stable des
outils, jamais une version épinglée sans raison.

---

## 11. Journal de décisions et interdits

### 11.1 Journal de décisions (ADR courts)

Créer `docs/decisions/ADR-000X-<sujet>.md` **au moment** de chaque décision structurante. Format
imposé, 15 lignes maximum, pas de dissertation :

```markdown
# ADR-0001 — <titre court>
Date : <AAAA-MM-JJ>
Statut : accepté / à revoir
Contexte : <2-3 lignes : ce qui oblige à décider>
Décision : <1-2 lignes : ce qui a été choisi>
Alternatives écartées : <1-2 lignes + pourquoi>
Conséquences : <ce que ça impose, y compris les limites assumées>
```

Décisions à consigner obligatoirement :

1. Option A (Astro) ou B (HTML/CSS/JS vanilla), et pourquoi.
2. Polices auto-hébergées ou Google Fonts limité, avec le poids engagé.
3. Solution de formulaire retenue (Netlify Forms ou `mailto:`) et conséquence RGPD.
4. Coordonnées géographiques utilisées (source du géocodage) ou absence de `geo`.
5. Rôle des couleurs de `NON RELEVÉES — aucun visuel récupérable (pages sociales derrière authentification). Extraire la palette du logo fourni par le client avec le skill `identite-visuelle`, puis la faire confirmer avant production. En attendant, ne pas inventer de teintes : proposer 3 pistes sobre et les soumettre au client.` quand les rôles n'étaient pas explicites.
6. Absence de bannière de cookies (aucun traceur déployé).
7. Tout arbitrage sur une variable ambiguë.
8. Toute omission volontaire d'un bloc ou d'une page (ex. `/avis` sans avis réels).

L'objectif : qu'un humain reprenne le site dans six mois et comprenne chaque choix en le lisant.

### 11.2 Ce que l'IA ne doit pas faire (récapitulatif)

- **Ne rien inventer** : pas de faux avis, faux chiffres, faux prix, fausse certification, fausse
  ancienneté, faux nom, faux horaire, fausse adresse, faux réseau social (§ 0.3).
- Ne pas inventer de communes desservies, de langue supplémentaire, de version anglaise, de page
  « blog », de page « promotions » qui n'existe pas dans le réel.
- Ne pas prendre une photo de banque d'images pour une photo du commerce ou d'un produit.
- Ne pas modifier la palette, les polices ou le logo fournis, ni créer de variante non autorisée.
- Ne pas ajouter de traceur, de cookie, d'iframe vidéo, de widget tiers, de service payant.
- Ne pas créer de page SEO vide ni de liste de villes pour capter du trafic.
- Ne pas retoucher le verbatim d'un avis (orthographe, sens, longueur, traduction).
- Ne pas promettre de délai de réponse, de garantie, de disponibilité non fourni.
- Ne pas laisser de contenu de démonstration (lorem ipsum, « Tel : 0123456789 »,
  « contact@exemple.fr », images `placeholder.png`) dans la livraison.
- Ne pas livrer un formulaire non fonctionnel présenté comme fonctionnel.
- Ne pas affirmer une conformité légale ou une certification d'accessibilité.
- Ne pas déployer en production sans vérification du build (`npm run build` + `preview` + recette).

### 11.3 Fin de mission

Quand § 10.1 est coché, produire un rapport final court : ce qui est livré, les commandes exécutées
et leurs résultats, les scores obtenus, ce qui reste à compléter par le client
(`docs/A-COMPLETES.md`) et les limites assumées. Un rapport qui annonce « terminé » sans les
résultats des commandes est invalide.

<!-- PROMPT — FIN -->
```

## Historique des régénérations

| Date | Version du skill `prompt-vitrine` | Ce qui a changé |
|---|---|---|
| 2026-09-29 | 1.0.0 | création (variables OSM + vérification en ligne) |

## Ce qui reste à vérifier

- [ ] Variables vides à obtenir du client : palette, typographies, logo, photos, email, SIRET, hébergeur.
- [ ] Sous-domaine : vérifier qu'il est libre (`la-tradition`).
- [ ] Relire le prompt avant livraison : adresse, téléphone, horaires exacts.
