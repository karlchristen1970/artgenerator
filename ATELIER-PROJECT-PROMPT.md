[ATELIER-PROJECT-PROMPT.md](https://github.com/user-attachments/files/26444478/ATELIER-PROJECT-PROMPT.md)
# Atelier — Giclée Print Studio
## Project Master Prompt & Session Continuity Document

---

## What This App Is

Atelier is a browser-based fine art giclée print generator. It takes a painting description, print dimensions in inches, DPI setting, and painting style — then generates a high-quality AI artwork via the Google Imagen 4 API, upscales it to the exact pixel dimensions required for professional giclée printing, applies a sharpening pass, and outputs a downloadable PNG.

**Live URL:** `https://karlchristen1970.github.io/artgenerator`

**GitHub Repo:** `https://github.com/karlchristen1970/artgenerator`
- Single file: `index.html` in root of repo
- To update: edit `index.html` directly in GitHub via pencil icon → commit changes
- GitHub Pages auto-redeploys in ~30 seconds after each commit

---

## Tech Stack

- Single-file HTML/CSS/JS — no build process, no dependencies, no framework
- **Image generation:** Google Imagen 4 API (`imagen-4.0-generate-001`) via REST
- **Fallback model:** Gemini 3.1 Flash image generation (`gemini-3.1-flash-image-preview`)
- **Upscaling:** Browser Canvas API with convolution sharpening kernel
- **Output:** PNG blob downloaded directly to device
- **Hosting:** GitHub Pages (free, https://, no server needed)

---

## API Configuration

- **Provider:** Google AI Studio
- **Key source:** aistudio.google.com → API Keys
- **Account tier:** Paid Tier 1 (karlchristen@gmail.com)
- **Primary model:** `imagen-4.0-generate-001` — best quality, paid tier
- **Fallback model:** `gemini-3.1-flash-image-preview` — if Imagen 4 fails
- **Key entry:** User pastes key into the app's API key field each session (not hardcoded)
- **Optional hardcode:** Find `<input type="password" id="apiKey"` and add `value="YOUR_KEY"` to pre-fill

---

## Current Features

- Width × Height input in inches (2–40", 0.5 increments)
- DPI selector: 240 / 300 / 360
- 8 painting styles: Oil, Watercolor, Acrylic, Pastel, Charcoal, Digital, Impressionist, Photorealistic
- Optional reference image upload (JPG/PNG/WEBP)
- Text prompt up to 1200 characters with live counter
- Aspect ratio auto-detection for Google API (1:1, 4:3, 3:4, 16:9, 9:16)
- Canvas upscale to exact target resolution with sharpening
- Two-pass upscale for prints over 40 megapixels
- PNG download with filename including dimensions and DPI
- Copy prompt button
- Output meta bar showing dimensions, resolution, pixel count, file size

---

## Design System

- **Aesthetic:** Dark atelier/gallery — luxury fine art studio feel
- **Background:** Near-black `#0e0c0a` with grain overlay
- **Accent:** Antique gold `#c9a84c` / `#8a6f2e`
- **Text:** Parchment `#f4f0e8`
- **Fonts:** Cormorant Garamond (serif display) + Montserrat (sans UI)
- **Layout:** Two-column — 420px control panel left, canvas area right

---

## Known Issues / History

- Pollinations.ai (original backend) was unreliable — replaced with Google Imagen 4
- CORS blocks all direct API calls from `file://` — must be deployed to `https://`
- GitHub Pages requires repo to be **public** to use free tier Pages
- Google model names changed mid-development:
  - Old broken name: `gemini-2.0-flash-preview-image-generation`
  - Current fallback: `gemini-3.1-flash-image-preview`
- AI-generated text in images (stadium signs, etc.) may have spelling errors — normal behavior

---

## Planned Upgrades (Next Session Priorities)

### 1. Negative Prompt Field
Add a "Exclude from image" text input that appends negative guidance to the prompt. Helps eliminate unwanted elements like bad text rendering, modern elements in period scenes, watermarks.

### 2. Generation History Panel
Save the last 10 generated images to IndexedDB (same pattern as Vault). Show thumbnails in a collapsible history drawer on the right side. Click any thumbnail to reload its prompt and settings.

### 3. Prompt Library / Favorites
Save frequently used prompts and style combinations with a custom name. One-click to reload. Stored in localStorage. Could include preset "Atelier Collections" — Americana, Landscapes, Portraits, etc.

### 4. Embedded DPI Metadata in PNG Output
Use a canvas trick or EXIF library to write the actual DPI value (pHYs chunk) into the PNG file so it opens correctly in Photoshop and print software at the right physical size.

### 5. Print Service Integration
Add a "Send to Printer" button that connects to a giclée print service API (e.g., Printful, Canvera, or Bay Photo). Pre-fill dimensions and file automatically.

### 6. Style Reference Weighting
When a reference image is uploaded, add a slider (0–100%) controlling how strongly the reference influences the output vs. the text prompt. Pass as a parameter to the API.

### 7. Batch Generation
Generate 3–4 variations of the same prompt simultaneously and show them in a grid. User picks the best one to upscale and download. Reduces trial-and-error.

### 8. Mobile-Optimized Layout
Current layout is desktop-first. Add a responsive breakpoint that stacks the panel above the canvas on mobile, with a collapsible controls drawer.

---

## How to Brief Claude for Updates

Start a new Claude session and paste this:

> "I'm working on the Atelier Giclée Print Studio — a single-file HTML app hosted at https://karlchristen1970.github.io/artgenerator with source at https://github.com/karlchristen1970/artgenerator. The file is a single index.html — no build process. It uses Google Imagen 4 API for image generation and Canvas API for upscaling to giclée print resolutions. Today I want to [DESCRIBE WHAT YOU WANT TO ADD OR FIX]. Please make the change and give me the updated file to paste into GitHub."

Then attach the current `index.html` file from GitHub if Claude needs to see the full code (click Raw → copy all → paste as attachment).

---

## Session Date
April 2, 2026 — Initial build session with Jon (Claude Sonnet 4.6)
