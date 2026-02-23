---
name: zilliz-brand-guidelines
description: >
  Applies Zilliz's official brand colors, typography, and visual identity to any generated
  artifact, document, presentation, or web content. Use this skill when creating materials
  that should follow Zilliz brand standards, including slides, HTML pages, React components,
  PDFs, blog covers, social media graphics, event banners, or any visual output requiring
  Zilliz's look-and-feel. Triggers include mentions of "Zilliz brand", "Zilliz style",
  "on-brand", "brand colors", "Zilliz template", or any request to produce Zilliz-branded
  content.
---

# Zilliz Brand Guidelines

This skill ensures all generated content adheres to Zilliz's official brand identity as
defined in the Zilliz Brand Guidelines & Style Guide (v1.0, August 2022).

## Company Identity

- **Name pronunciation**: /ˈziliz/ — coined by founders, standing for "zillions of zillions" of unstructured data
- **Mission**: Building the vector database for production-ready AI
- **About**: Zilliz is built by the engineers and scientists who created LF AI Milvus®, an open-source vector database. On a mission to unleash data insights with AI, Zilliz builds database and search technologies for AI/ML applications.

## Logo

### Core Logo
The Zilliz logo consists of a **logomark** (star symbol) and **logotype** (wordmark). This is the primary logo and must be used without modification.

### Symbol (Logomark)
- 12 lines of different lengths representing vector coordinates, resembling a twinkling star
- Mathematically and geometrically constructed based on a coordinate system
- Key angles: 20°, 30°, 40° with lengths at 60%, 90%, 100%
- **Never** change angles or recreate the symbol — only use provided artwork files

### Clear Space
- Margins determined by half the height of the symbol (0.5X on sides, X on top/bottom)

### Minimum Size
- Full logo: minimum height 24px
- Recommended sizes: 120px, 72px, 48px

### Logo Color Variants
1. **Gradient** (preferred, especially on digital media) — the gradient is Zilliz's unique identity
2. **Black**: RGB 0,0,0 / HEX #000000
3. **White**: RGB 255,255,255 / HEX #FFFFFF (reverse/knockout version)

### Logo Usage Rules — NEVER:
- Fade out the logo
- Alter the colors
- Stretch the logo
- Place the logo over a busy image
- Apply drop shadows
- Rotate the logo

## Typography

### Primary Font: Inter
- **Download**: https://fonts.google.com/specimen/Inter
- A modernistic sans-serif designed for digital channels
- **Primary weights**: Regular (400) and Medium (500)
- **Accent weight**: Bold (700) — use for contrast and highlighting
- Available weights: Light, Regular, Medium, SemiBold, Bold

### Monospace Font: IBM Plex Mono
- **Download**: https://fonts.google.com/specimen/IBM+Plex+Mono
- A neutral, friendly grotesque typeface designed for coding environments
- Represents the relationship between mankind and machine
- **Primary weight**: Regular (400)

### Typography Application Rules
- Use Inter for all headings, body text, and UI elements
- Use IBM Plex Mono for code snippets, technical data, and terminal-style content
- Import via Google Fonts for web: `@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600;700&display=swap');`

## Colors

### Primary Colors

| Name  | HEX       | RGB            | Usage                        |
|-------|-----------|----------------|------------------------------|
| Blue  | `#175FFF` | 23, 95, 255    | Primary brand color, CTAs    |
| Black | `#000000` | 0, 0, 0        | Text, dark backgrounds       |
| White | `#FFFFFF` | 255, 255, 255  | Backgrounds, reverse text    |

### Secondary Colors

| Name      | HEX       | RGB            | Usage                              |
|-----------|-----------|----------------|------------------------------------|
| Berry     | `#C84CFF` | 200, 76, 255  | Accents, gradients                 |
| Purple    | `#7F47FF` | 127, 71, 255  | Accents, gradients                 |
| Sky Blue  | `#49BCFF` | 73, 188, 255  | Highlights, secondary elements     |
| Green     | `#00DCC6` | 0, 220, 198   | Success states, accents            |
| Navy Blue | `#061982` | 6, 25, 130    | Deep backgrounds, contrast text    |

### Brand Gradient
The signature Zilliz gradient flows from **cyan/sky blue** → **blue** (#175FFF) → **purple** (#7F47FF) → **berry** (#C84CFF). This is used prominently in:
- Logo presentations
- Background treatments
- Business cards and swag
- Blog covers and social media

**CSS gradient example** (horizontal):
```css
background: linear-gradient(135deg, #49BCFF 0%, #175FFF 30%, #7F47FF 65%, #C84CFF 100%);
```

**CSS gradient example** (Zilliz hero-style):
```css
background: linear-gradient(135deg, #80F0E0 0%, #49BCFF 20%, #175FFF 45%, #7F47FF 75%, #C84CFF 100%);
```

## Application Guidelines

### HTML / React Artifacts
When generating HTML or React components with Zilliz branding:

```css
:root {
  /* Primary */
  --zilliz-blue: #175FFF;
  --zilliz-black: #000000;
  --zilliz-white: #FFFFFF;

  /* Secondary */
  --zilliz-berry: #C84CFF;
  --zilliz-purple: #7F47FF;
  --zilliz-sky-blue: #49BCFF;
  --zilliz-green: #00DCC6;
  --zilliz-navy: #061982;

  /* Gradient */
  --zilliz-gradient: linear-gradient(135deg, #49BCFF 0%, #175FFF 30%, #7F47FF 65%, #C84CFF 100%);

  /* Typography */
  --font-primary: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-mono: 'IBM Plex Mono', monospace;
}

body {
  font-family: var(--font-primary);
  font-weight: 400;
  color: var(--zilliz-black);
}

h1, h2, h3 {
  font-family: var(--font-primary);
  font-weight: 600;
}

code, pre {
  font-family: var(--font-mono);
  font-weight: 400;
}
```

### Presentations (PPTX)
- Use Zilliz Blue (#175FFF) as the primary slide accent color
- Title slides: use the brand gradient as background with white text
- Content slides: white background, black text, blue accents
- Font: Inter for all text (embed or use system sans-serif fallback)
- Code blocks: IBM Plex Mono

### Documents (DOCX / PDF)
- Headings: Inter SemiBold or Bold, Zilliz Blue (#175FFF)
- Body: Inter Regular, Black (#000000)
- Accent elements: Use blue (#175FFF) for horizontal rules, highlights
- Code: IBM Plex Mono Regular

### Blog Covers and Social Media
- Use the brand gradient as background
- White Zilliz logo in upper-left
- Title text in white, Inter SemiBold
- Speaker/author photos in circular frames with white borders
- Include thin decorative arc lines in white (characteristic of Zilliz visual style)

### Dark Theme Variants
For dark backgrounds (Navy #061982 or Black #000000):
- Use white logo and text
- Accent with blue-to-purple gradient lines (vector-style diagonal lines)
- These represent the "vector coordinates" brand motif

### Button Styles
- Primary CTA: Zilliz Blue (#175FFF) background, white text, rounded corners
- Hover: slightly darker blue or gradient treatment
- Secondary: white background, blue border, blue text

## Key Visual Motifs

1. **Vector coordinate lines**: Diagonal lines in blue/purple/pink representing vectors — used in roll-up banners, virtual backgrounds, dark-theme materials
2. **Gradient star symbol**: The logomark rendered with the brand gradient on circular backgrounds — used for badge pins, stickers, app icons
3. **Thin arc lines**: Subtle curved white lines overlaid on gradient backgrounds — used in blog covers and event materials

## Quick Reference — Color Codes

```
Blue:      #175FFF  rgb(23, 95, 255)
Black:     #000000  rgb(0, 0, 0)
White:     #FFFFFF  rgb(255, 255, 255)
Berry:     #C84CFF  rgb(200, 76, 255)
Purple:    #7F47FF  rgb(127, 71, 255)
Sky Blue:  #49BCFF  rgb(73, 188, 255)
Green:     #00DCC6  rgb(0, 220, 198)
Navy Blue: #061982  rgb(6, 25, 130)
```
