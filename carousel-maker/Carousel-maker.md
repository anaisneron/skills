---
name: carousel-maker
description: >
  Generate a branded, self-contained HTML carousel maker app for Instagram & LinkedIn.
  Reads a brand-config.json to apply any company's visual identity (colors, fonts, logo)
  and produces a ready-to-use local app with Apify-powered URL extraction (TikTok, YouTube, blog posts),
  slide editor, and PNG export at 1080x1350 (4:5 portrait).
  MANDATORY TRIGGERS: "carousel", "carrousel", "carousel maker", "carousel app",
  "Instagram carousel", "LinkedIn carousel", "slide maker", "social media slides",
  "content repurposing tool", "TikTok to carousel", "video to carousel", "blog to carousel",
  "create carousel tool", "build carousel app", or any request to turn content into
  social media carousel slides. Also trigger when the user asks to generate, customize,
  or deploy a carousel-making tool for themselves or a client.
---

# Carousel Maker Skill

Build a fully branded, self-contained HTML carousel maker app that anyone can run locally. The app turns TikTok videos, YouTube videos, and blog posts into polished Instagram/LinkedIn carousels — complete with automatic content extraction, a visual slide editor, and PNG export.

## How This Skill Works

This skill generates a **complete carousel maker app** customized to a specific brand. It’s not a one-off carousel — it’s a reusable tool that the end user opens in their browser whenever they need to create carousel content.

The generation pipeline:

1. Read `brand-config.json` from this skill’s directory (or ask the user to provide brand info)
2. Generate the full HTML app with that brand’s colors, fonts, logo, and identity baked in
3. Generate a macOS/Linux launcher script so the user can double-click to start
4. Deliver both files to the user’s output folder

## Brand Configuration

The skill looks for `brand-config.json` in its own directory. If it doesn’t exist, ask the user for brand details and create it. Here’s the schema:

\`\`\`
brand-config.json location: <this-skill-directory>/brand-config.json
\`\`\`

Read `references/brand-config-example.json` for the complete schema with annotations. The key fields are:

| Field | Required | Purpose |
|-------|----------|---------|
| `company.name` | Yes | Shown in header, footer watermark, exports |
| `company.tagline` | No | Subtitle in the app header |
| `colors.primary` | Yes | Headlines, dark backgrounds, body text |
| `colors.accent` | Yes | CTAs, highlights, active states, key data |
| `colors.background` | Yes | Page backgrounds, light sections |
| `colors.gradientStart` | No | Left/top of gradient backgrounds |
| `colors.gradientEnd` | No | Right/bottom of gradient backgrounds |
| `colors.textSecondary` | No | Captions, metadata, muted text |
| `colors.highlightBg` | No | Soft highlight backgrounds |
| `fonts.heading` | Yes | Font family for titles (with fallbacks) |
| `fonts.body` | Yes | Font family for body text (with fallbacks) |
| `fonts.googleFontsUrl` | No | Google Fonts import URL if using web fonts |
| `logo.svgMarkup` | No | Inline SVG for the logo (used in header + watermark) |
| `logo.showInFooter` | No | Whether to show brand watermark on slides |
| `apify.defaultToken` | No | Pre-filled Apify token (user can change in-app) |
| `export.defaultRatio` | No | "4:5" or "1:1" |
| `export.filePrefix` | No | Prefix for exported PNGs |
| `language` | No | "fr" or "en" — controls all UI labels |

## Generating the App

When generating the carousel maker, follow these steps exactly:

### Step 1: Load Brand Config

\`\`\`
Read <this-skill-directory>/brand-config.json
\`\`\`

If the file doesn’t exist, interview the user:
- Company name?
- Primary color (dark, for text/backgrounds) and accent color (bright, for CTAs)?
- Font preferences (serif headings + sans-serif body recommended)?
- Do they have an SVG logo they want embedded?
- Do they have an Apify API token?
- Preferred language (French or English)?

Then create `brand-config.json` from their answers.

### Step 2: Generate the HTML

Read the template structure from `assets/carousel-template.html`. This is the reference implementation — use it as the base and inject the brand config values into it.

Key injection points (marked with `{{PLACEHOLDER}}` in the template):

- `{{COMPANY_NAME}}` — company name in header and watermark
- `{{COMPANY_TAGLINE}}` — subtitle in header
- `{{COLOR_PRIMARY}}` — CSS custom property `--brand-primary`
- `{{COLOR_ACCENT}}` — CSS custom property `--brand-accent`
- `{{COLOR_BACKGROUND}}` — CSS custom property `--brand-bg`
- `{{COLOR_GRADIENT_START}}` — CSS custom property `--brand-gradient-start`
- `{{COLOR_GRADIENT_END}}` — CSS custom property `--brand-gradient-end`
- `{{COLOR_TEXT_SECONDARY}}` — CSS custom property `--brand-text-secondary`
- `{{COLOR_HIGHLIGHT_BG}}` — CSS custom property `--brand-highlight-bg`
- `{{FONT_HEADING}}` — CSS custom property `--font-heading`
- `{{FONT_BODY}}` — CSS custom property `--font-body`
- `{{GOOGLE_FONTS_URL}}` — href in the Google Fonts `<link>` tag
- `{{LOGO_SVG}}` — inline SVG in header and slide watermark
- `{{APIFY_TOKEN}}` — pre-filled token (or empty string)
- `{{FILE_PREFIX}}` — export filename prefix
- `{{LANG_*}}` — UI labels (see language section below)

### Step 3: Generate the Launcher

Create a `.command` file (macOS) that:
1. `cd`s to the directory containing the HTML file
2. Finds a free port starting at 8080
3. Opens the browser automatically
4. Starts `python3 -m http.server` on that port

Name it using the company name: `lancer-carrousel-{company}.command` (FR) or `launch-carousel-{company}.command` (EN).

### Step 4: Deliver

Save both files to the user’s output directory and present them.

## Language Support

The app UI supports French and English. The `language` field in brand-config.json controls which set of labels is used. When generating the HTML, replace all `{{LANG_*}}` placeholders with the appropriate strings:

| Placeholder | French (fr) | English (en) |
|------------|-------------|--------------|
| `{{LANG_TITLE}}` | Carrousel Maker | Carousel Maker |
| `{{LANG_SOURCE}}` | Source | Source |
| `{{LANG_SLIDES}}` | Diapos | Slides |
| `{{LANG_STYLE}}` | Style | Style |
| `{{LANG_EXTRACT}}` | Extraire | Extract |
| `{{LANG_EXTRACT_AUTO}}` | Extraire automatiquement | Extract automatically |
| `{{LANG_PASTE_MANUAL}}` | Ou coller manuellement | Or paste manually |
| `{{LANG_PASTE_PLACEHOLDER}}` | Colle ici le texte... | Paste your text here... |
| `{{LANG_CREATE_SLIDES}}` | Créer les diapos | Create slides |
| `{{LANG_GENERATE_AUTO}}` | Générer les diapos automatiquement | Auto-generate slides |
| `{{LANG_EXPORT_PNG}}` | Exporter en PNG | Export as PNG |
| `{{LANG_CONNECT_APIFY}}` | Connecter Apify | Connect Apify |
| `{{LANG_CONNECTED}}` | Apify connecté | Apify connected |
| `{{LANG_COVER}}` | Couverture | Cover |
| `{{LANG_CONTENT}}` | Contenu | Content |
| `{{LANG_LIST}}` | Liste à puces | Bullet list |
| `{{LANG_QUOTE}}` | Citation | Quote |
| `{{LANG_STAT}}` | Statistique | Statistic |
| `{{LANG_CTA}}` | Appel à l’action | Call to action |
| `{{LANG_PREV}}` | ← Précédente | ← Previous |
| `{{LANG_NEXT}}` | Suivante → | Next → |
| `{{LANG_EDIT_TAG}}` | Étiquette / sous-titre | Tag / subtitle |
| `{{LANG_EDIT_TITLE}}` | Titre principal | Main title |
| `{{LANG_EDIT_BODY}}` | Corps du texte | Body text |
| `{{LANG_EDIT_AUTHOR}}` | Auteur / Source | Author / Source |
| `{{LANG_THEME}}` | Thème de la diapo | Slide theme |
| `{{LANG_LAYOUT}}` | Mise en page | Layout |
| `{{LANG_CTA_DEFAULT}}` | Prêt à passer à l’action? | Ready to take action? |
| `{{LANG_CTA_BODY}}` | Abonne-toi pour plus de conseils! | Follow for more tips! |
| `{{LANG_SWIPE}}` | Glisse pour découvrir → | Swipe to discover → |

## Apify Integration Details

The app calls the Apify REST API directly from the browser (requires localhost, not file://). Three extraction paths:

**TikTok** → `clockworks~tiktok-scraper` with `downloadSubtitlesOptions: "DOWNLOAD_AND_TRANSCRIBE_VIDEOS_WITHOUT_SUBTITLES"`. Extracts transcript from subtitles/transcription, fell back to caption.

**YouTube** → `apify~rag-web-browser` with the video URL. Extracts page text content.

**Blog/Article** → `apify~rag-web-browser` with the article URL. Extracts full text.

Important: actor names use `~` (not `/`) in REST API URLs. The app must encode this correctly:
\`\`\`javascript
function encodeActorId(id) { return id.replace('/', '~'); }
\`\`\`

The API flow: POST to start run → poll for completion → GET dataset items → parse content → auto-split into slides.

## Slide Architecture

6 layout types, 5 themes. All slides render at 1080x1350px (4:5 portrait) and scale down for preview.

**Layouts:** cover, content, list, quote, stat, cta
**Themes:** primary (dark bg), white, muted (light bg), gradient, accent (bright bg)

Theme colors map to the brand config — "primary" uses `colors.primary`, "accent" uses `colors.accent`, etc. The template handles all the CSS logic.

## File Structure

\`\`\`
carousel-maker/
├── SKILL.md                              ← you are here
├── brand-config.json                     ← company-specific (created per client)
├── assets/
│   └── carousel-template.html            ← the full HTML app template
└── references/
    ├── brand-config-example.json         ← annotated example config
    └── SETUP-GUIDE.md                    ← walkthrough for end users
\`\`\`
