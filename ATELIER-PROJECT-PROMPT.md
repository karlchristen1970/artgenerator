# Atelier — Giclée Print Studio
## Project Master Prompt & Session Continuity Document

---

## What This App Is

Atelier is a browser-based fine art giclée print studio with two modes:

**Generate Mode** — takes a painting description, optional reference photo, print dimensions in inches, DPI setting, and painting style — then generates a high-quality AI artwork via the Google Imagen 4 API (with Gemini fallback), upscales it to the exact pixel dimensions required for professional giclée printing, applies a sharpening pass, and outputs a downloadable PNG.

**Upscale Mode** — takes a photo of an existing original painting, upscales it to exact giclée print resolution using either a Faithful (canvas-only, no AI, no API key required) or AI Enhanced (Gemini detail recovery + upscale) method. Built specifically for painters who want to produce print-ready files from their original work.

**Live URL:** `https://karlchristen1970.github.io/artgenerator`

**GitHub Repo:** `https://github.com/karlchristen1970/artgenerator`
- Single file: `index.html` in root of repo
- To update: edit `index.html` directly in GitHub via pencil icon → commit changes
- GitHub Pages auto-redeploys in ~30 seconds after each commit

---

## How to Copy Updated Code into GitHub (Windows)

1. Open the downloaded `index.html` in Chrome
2. Press **Ctrl+U** — opens page source in a new tab
3. Press **Ctrl+A** — selects all code
4. Press **Ctrl+C** — copies it
5. Go to `github.com/karlchristen1970/artgenerator`
6. Click `index.html` in the file list
7. Click the **pencil icon** (Edit)
8. Press **Ctrl+A** to select all existing code
9. Press **Ctrl+V** to paste
10. Click **Commit changes**

---

## Tech Stack

- Single-file HTML/CSS/JS — no build process, no dependencies, no framework
- **Image generation:** Google Imagen 4 API (`imagen-4.0-generate-001`) via REST
- **Fallback models:** `gemini-2.0-flash-preview-image-generation` → `gemini-3.1-flash-image-preview`
- **Upscaling:** Browser Canvas API with convolution sharpening kernel
- **Storage:** None — all processing is in-browser, nothing leaves the user's machine
- **Output:** PNG blob (print) or JPEG blob (web export) downloaded directly to device
- **Hosting:** GitHub Pages (free, https://, no server needed)

---

## API Configuration

- **Provider:** Google AI Studio
- **Key source:** aistudio.google.com → API Keys
- **Account tier:** Paid Tier 1 (karlchristen@gmail.com)
- **Primary model:** `imagen-4.0-generate-001` — text-to-image, best quality, paid tier
- **Multimodal model:** `gemini-2.0-flash-preview-image-generation` — image+text, used when reference photo is provided
- **Secondary fallback:** `gemini-3.1-flash-image-preview`
- **Key entry:** User pastes key into app's API key field each session (not hardcoded)
- **Optional hardcode:** Find `<input type="password" id="apiKey"` and add `value="YOUR_KEY"` to pre-fill for shared/personal use
- **Note:** Faithful upscale requires no API key — runs entirely in the browser

---

## Design System

- **Aesthetic:** Cool modern gallery — deep navy, slate panels, silver-blue accents
- **Background:** Deep navy `#080d18`
- **Panel background:** `rgba(8,13,24,0.6)`
- **Primary accent:** Silver-blue `#9ab0cc`
- **Accent light:** `#c4d4e8`
- **Accent dim:** `#5a7a96`
- **Text:** Cool parchment `#e4eaf4`
- **Muted:** `#5a6e85`
- **Border:** `rgba(154,176,204,0.25)`
- **Fonts:** Cormorant Garamond (serif display) + Montserrat (sans UI)
- **Layout:** Two-column — 420px control panel left, canvas area right
- **Grain overlay:** SVG fractalNoise at 35% opacity for texture depth

---

## Current Features

### Generate Mode
- Width × Height input in inches (2–40", 0.5 increments)
- DPI selector: 150 / 240 / 300 (default 240)
- Smart DPI hint — contextual guidance appears when DPI button is clicked
- 8 painting styles: Oil, Watercolor, Acrylic, Pastel, Charcoal, Impasto, Impressionist, Textured Giclée
- Optional reference image upload — **actually wired to API** via Gemini multimodal (image + text → painting)
- When reference image is loaded: description field becomes optional with updated placeholder
- Text prompt up to 1200 characters with live counter
- All prompts include no-frame clause: `no frame, no border, no mat, no vignette, painting surface only, full bleed composition`
- Auto-fallback prompt when description is blank but reference image is present
- Aspect ratio auto-detection for Google API (1:1, 4:3, 3:4, 16:9, 9:16)
- Canvas upscale to exact target resolution with sharpening
- Two-pass upscale for prints over 40 megapixels
- Copy Prompt button
- Output meta bar showing Mode, Dimensions, Resolution, Pixels, File Size, Format

### Upscale Mode
- Large drop zone for photographer's original painting
- **Faithful** — pure canvas scale + sharpen, no API call, works without API key, zero alteration
- **AI Enhanced** — sends painting to Gemini with archival detail-recovery prompt, then upscales result
- Live method description updates when toggle is switched
- Same Dimensions + DPI inputs as Generate mode

### Shared / Output
- **Download Print PNG** — full resolution archival PNG at target DPI
- **Save for Web** — JPEG at 85% quality, max 1500px long edge, for listings and web use
- Output meta bar labels mode (Generated / Faithful Upscale / AI Enhanced Upscale)
- Reset / New Print button

---

## Painting Styles & Prompt Keywords

| Style | Prompt Emphasis |
|---|---|
| Oil | Impasto brushwork, layered glazes, old masters, luminous highlights |
| Watercolor | Wet-on-wet blooms, translucent washes, granulation, cold press paper |
| Acrylic | Bold saturated color, contemporary, textured brushwork |
| Pastel | Chalky matte surface, blended tones, velvety finish |
| Charcoal | Dramatic tonal range, expressive gestural marks, deep blacks |
| Impasto | Extremely thick application, palette knife, raised ridges, Van Gogh/de Kooning influence |
| Impressionist | Broken color, visible brushstrokes, plein air, Monet/Pissarro |
| Textured Giclée | Canvas weave beneath archival inks, matte substrate tooth, linen grain |

---

## Known Issues / History

- Pollinations.ai (original backend) was unreliable — replaced with Google Imagen 4
- CORS blocks all direct API calls from `file://` — must be deployed to `https://`
- GitHub Pages requires repo to be **public** for free tier Pages
- Reference image field was previously decorative (not sent to API) — **fixed in Session 1**
- Gemini image generation models tend to add gallery frames around artwork — **fixed in Session 1** via no-frame prompt clause
- AI-generated text in images may have spelling errors — normal model behavior
- AI Enhanced upscale interprets/reconstructs detail — output is not pixel-identical to source

---

## Planned Upgrades (Future Sessions)

### 1. Generation History Panel
Save last 10 generated images to IndexedDB (same pattern as Vault). Collapsible history drawer on the right. Click thumbnail to reload prompt and settings.

### 2. Prompt Library / Favorites
Save frequently used prompts and style combos with a custom name. One-click reload. localStorage. Could include preset Atelier Collections — Americana, Landscapes, Portraits, etc.

### 3. Embedded DPI Metadata in PNG Output
Write actual DPI value (pHYs chunk) into the PNG file so it opens at correct physical size in Photoshop and print software.

### 4. Print Service Integration
"Send to Printer" button connecting to a giclée print service API (Printful, Canvera, Bay Photo). Pre-fill dimensions and file automatically.

### 5. Style Reference Weighting
When reference image is uploaded, slider (0–100%) controls how strongly it influences output vs. text prompt.

### 6. Batch Generation
Generate 3–4 variations simultaneously, show in a grid. User picks best one to upscale and download.

### 7. Mobile-Optimized Layout
Responsive breakpoint stacking panel above canvas on mobile with collapsible controls drawer.

### 8. Negative Prompt Field
"Exclude from image" input that appends negative guidance to the prompt. Eliminates unwanted elements like bad text, modern elements in period scenes, watermarks.

---

## Personal Context

- **Primary use case:** Personal production tool for Numivas (numismatic display platform) and Knight Gallery
- **Knight Gallery:** Original paintings by Laura Knight — landscapes with Bigfoot/Yeti as signature easter egg. Sold on eBay. Atelier enables giclée print production from her originals for additional revenue stream.
- **API costs:** Running on personal Google AI Studio key at personal volume — no enterprise agreement needed at current scale
- **Architecture note:** Current single-key architecture is intentional for personal use. Any future public/commercial version would require backend, user accounts, and API cost management.

---

## How to Brief Claude for Updates

Start a new session and paste this document, then add:

> "I'm working on the Atelier Giclée Print Studio — a single-file HTML app at https://karlchristen1970.github.io/artgenerator with source at https://github.com/karlchristen1970/artgenerator. Today I want to [DESCRIBE WHAT YOU WANT]. Please make the change and give me the updated file."

Attach the current `index.html` if Claude needs to see the full code (GitHub → index.html → Raw → copy all).

---

## Session Log

**Session 1 — April 2, 2026** with Jon (Claude Sonnet 4.6)
- Initial build with Google Imagen 4 + Gemini fallback
- Color system redesigned: warm gold/parchment → cool navy/slate/silver
- DPI options changed from 240/300/360 → 150/240/300
- Painting styles: replaced Digital + Photorealistic with Impasto + Textured Giclée
- Added Generate / Upscale mode toggle
- Fixed reference image — now actually sent to Gemini multimodal API
- Added Faithful upscale (no API, browser-only)
- Added AI Enhanced upscale (Gemini detail recovery)
- Added Save for Web export (JPEG, 1500px max, 85% quality)
- Fixed frame generation issue — added no-frame clause to all prompts
- Made description optional when reference image is loaded
- Added smart DPI contextual hints
