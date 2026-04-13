# Carousel Maker — Guide de configuration

Ce guide explique comment configurer le Carousel Maker pour ton entreprise. En 15 minutes, tu auras un outil de création de carrousels Instagram/LinkedIn entièrement brandé à tes couleurs.

---

## Ce dont tu as besoin

1. **Claude Desktop** (ou Claude Code) avec les skills activés
2. **Tes couleurs de marque** (2-3 codes hex minimum)
3. **Tes polices de caractères** (ou utilise les suggestions par défaut)
4. **Ton logo en SVG** (optionnel mais recommandé)
5. **Un token Apify** (gratuit, pour l'extraction automatique de contenu)
6. **Python 3** installé sur ton ordinateur (pour le serveur local)

---

## Étape 1 : Installer le skill

Copie le dossier \`carousel-maker/\` dans le répertoire de skills de ton projet Claude :

\`\`\`
.claude/skills/carousel-maker/
├── SKILL.md
├── brand-config.json        ← tu vas créer ce fichier
├── assets/
│   └── carousel-template.html
└── references/
    ├── brand-config-example.json
    └── SETUP-GUIDE.md       ← tu es ici
\`\`\`

---

## Étape 2 : Créer ton brand-config.json

Copie \`references/brand-config-example.json\` vers la racine du skill et renomme-le \`brand-config.json\`. Puis personnalise chaque section :

### Nom et slogan

\`\`\`json
"company": {
  "name": "Ton Entreprise",
  "tagline": "Ton slogan ici"
}
\`\`\`

### Couleurs

Tu as besoin d'au minimum 2 couleurs : une primaire (foncée) et un accent (vive). Les autres sont optionnelles.

\`\`\`json
"colors": {
  "primary":        "#1C2B4A",   ← Couleur foncée : titres, fonds sombres, texte
  "accent":         "#C8102E",   ← Couleur vive : boutons, liens, accents
  "background":     "#F5F0EC",   ← Fond de page clair
  "gradientStart":  "#E8E4EF",   ← Dégradé gauche/haut (optionnel)
  "gradientEnd":    "#F0E0E0",   ← Dégradé droit/bas (optionnel)
  "textSecondary":  "#6B7280",   ← Texte secondaire, légendes
  "highlightBg":    "#FDE8E8"    ← Fond d'accent doux
}
\`\`\`

**Comment trouver tes couleurs :**
- Ouvre ton site web et utilise l'inspecteur Chrome (clic droit > Inspecter) pour repérer les codes hex
- Ou demande à ton graphiste / consulte ta charte graphique
- Ou utilise [coolors.co](https://coolors.co) pour générer une palette

### Polices

\`\`\`json
"fonts": {
  "heading":        "'Montserrat', Arial, sans-serif",
  "body":           "'Inter', Calibri, sans-serif",
  "googleFontsUrl": "https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700&family=Inter:wght@300;400;600&display=swap"
}
\`\`\`

**Comment construire ton URL Google Fonts :**
1. Va sur [fonts.google.com](https://fonts.google.com)
2. Cherche et sélectionne ta police de titres (poids 400, 600, 700)
3. Cherche et sélectionne ta police de corps (poids 300, 400, 600)
4. Clique "Get embed code" > "@import" et copie l'URL

**Combinaisons populaires :**
- \`Playfair Display\` (titres) + \`Open Sans\` (corps) — élégant
- \`Montserrat\` (titres) + \`Inter\` (corps) — moderne
- \`Poppins\` (titres) + \`Lato\` (corps) — friendly
- \`Raleway\` (titres) + \`Roboto\` (corps) — tech

### Logo (optionnel)

Si tu as un logo SVG, colle le code SVG directement :

\`\`\`json
"logo": {
  "svgMarkup": "<svg viewBox='0 0 100 100' width='36' height='36'>...</svg>",
  "showInFooter": true
}
\`\`\`

**Comment obtenir ton SVG :**
- Exporte depuis Figma/Illustrator en SVG
- Ou utilise [svgomg](https://jakearchibald.github.io/svgomg/) pour optimiser
- Le SVG doit utiliser \`currentColor\` comme couleur pour s'adapter automatiquement aux thèmes clairs/foncés

Si tu n'as pas de logo SVG, laisse le champ vide — l'app affichera simplement le nom de l'entreprise.

### Token Apify

\`\`\`json
"apify": {
  "defaultToken": "apify_api_ton_token_ici"
}
\`\`\`

Le token sera pré-rempli dans l'app mais modifiable par l'utilisateur. Pour obtenir un token :

1. Crée un compte gratuit sur [apify.com](https://apify.com)
2. Va dans **Settings > Integrations**
3. Copie ton **Personal API Token**

Le forfait gratuit inclut 5$ de crédits/mois — suffisant pour environ 500 extractions TikTok.

### Préférences d'export

\`\`\`json
"export": {
  "defaultRatio": "4:5",
  "filePrefix": "carrousel-monentreprise"
}
\`\`\`

### Langue

\`\`\`json
"language": "fr"
\`\`\`

Options : \`"fr"\` (français) ou \`"en"\` (anglais). Contrôle tous les textes de l'interface.

---

## Étape 3 : Générer ton app

Une fois le \`brand-config.json\` rempli, demande à Claude :

> "Génère mon carousel maker avec mon branding"

ou

> "Create my branded carousel maker"

Claude va lire ton \`brand-config.json\` et générer :
1. **carousel-maker.html** — l'application complète
2. **lancer-carrousel.command** — le lanceur macOS

Les deux fichiers seront déposés dans ton dossier de travail.

---

## Étape 4 : Lancer l'app

### macOS
Double-clique sur \`lancer-carrousel.command\`. Si macOS bloque l'exécution :
1. Clic droit sur le fichier
2. Clique **Ouvrir**
3. Confirme **Ouvrir** dans la boîte de dialogue

Le terminal s'ouvre, le serveur démarre, et ton navigateur ouvre l'app automatiquement.

### Linux
\`\`\`bash
cd /chemin/vers/le/dossier
python3 -m http.server 8080
# Puis ouvre http://localhost:8080/carousel-maker.html
\`\`\`

### Windows
\`\`\`cmd
cd C:\\chemin\\vers\\le\\dossier
python -m http.server 8080
:: Puis ouvre http://localhost:8080/carousel-maker.html
\`\`\`

---

## Utilisation de l'app

### Créer un carrousel depuis un URL

1. Colle l'URL d'un TikTok, d'une vidéo YouTube ou d'un article de blogue
2. Clique **Extraire** — l'app récupère automatiquement le contenu
3. Clique **Générer les diapos automatiquement**
4. Modifie chaque diapo dans le panneau d'édition
5. Ajuste les thèmes et mises en page dans l'onglet Style
6. Clique **Exporter en PNG**

### Créer un carrousel manuellement

1. Colle ton texte dans la zone de saisie manuelle
2. Clique **Créer les diapos** — le texte est découpé intelligemment
3. Modifie, stylise, exporte

### Importer un JSON prédéfini

Tu peux aussi coller un JSON structuré directement :

\`\`\`json
[
  { "tag": "", "title": "Mon titre", "body": "Glisse →", "layout": "cover", "theme": "primary" },
  { "tag": "Point #1", "title": "Conseil", "body": "Détails ici.", "layout": "content", "theme": "white" },
  { "tag": "", "title": "Abonne-toi!", "body": "Lien en bio", "layout": "cta", "theme": "primary" }
]
\`\`\`

---

## Raccourcis clavier

| Touche | Action |
|--------|--------|
| ← | Diapo précédente |
| → | Diapo suivante |

---

## Dépannage

| Problème | Solution |
|----------|----------|
| "Failed to fetch" | Ouvre l'app via \`localhost\`, pas en double-cliquant le HTML |
| "Apify API 401" | Token invalide — vérifie dans Settings > Integrations sur apify.com |
| "Apify API 402" | Crédits Apify épuisés — upgrade ton forfait ou attends le renouvellement |
| Polices pas chargées | Vérifie l'URL Google Fonts dans ton brand-config.json |
| Logo invisible | Assure-toi que le SVG utilise \`currentColor\` et non des couleurs hardcodées |
| Export PNG flou | Normal en preview — l'export est en 1080x1350 pleine résolution |

---

## Mettre à jour ton branding

Pour modifier les couleurs, polices ou logo :
1. Édite \`brand-config.json\`
2. Demande à Claude de régénérer l'app
3. Remplace les anciens fichiers

L'app elle-même ne lit pas le JSON au runtime — le branding est intégré au HTML lors de la génération. Il faut donc régénérer à chaque changement de marque.
