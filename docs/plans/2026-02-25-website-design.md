# Black Opal — Investor Pitch Website Design

## Date: 2026-02-25

## Purpose

Investor/partner pitch site. A cinematic scroll experience that progressively reveals the Black Opal concept, forks into two parallel formats (hotel and desert), and ends with a contact form and gated section for deeper materials.

## Architecture

Single `index.html` file. No build tools. Embedded CSS and JS. WebGL-powered visuals. Deployable anywhere.

## Scroll Structure

### ACT I — SHARED ORIGIN

1. **The Portal** (hero)
   - Black void. WebGL particle shader coalesces into the Black Opal logo.
   - Signature gradient palette (black -> magenta -> orange -> gold).
   - Text fades in: "An intelligent species has been evolving beside us..."

2. **The Reveal**
   - Scroll-driven text reveals: "For one night they invite us inside" -> "What we find changes how we see the future."
   - Particle field responds to scroll position (color, density shifts).

3. **The Vision** — "What is Black Opal?"
   - Core concept in cinematic prose. Immersive experience, AI tribes, robotics + performers, the emotional arc.
   - Brand-level storytelling shared by both formats.
   - Key lines from deck: "Not entertainment you watch. Entertainment that surrounds you."

4. **The Tribes**
   - Four animated sections, one per tribe:
     - **The Luminous Order** — elegance, symmetry, perfection (light rays, geometric particles)
     - **The Wild Current** — chaos, color, emotional force (turbulent, colorful particles)
     - **The Reclaimers** — recycled tech, adaptive survivalists (fragmented geometry, glitch effects)
     - **The Human Witnesses** — us (subtle, organic movement)
   - Each tribe gets a generative visual identity and short description.

### THE FORK — "Two Stages. One Dance."

5. **The Fork**
   - Visual divergence moment. Two paths separate on screen.
   - Left: warm interior glow (hotel). Right: vast open horizon (desert).
   - Shared statement: "The Dance takes two forms."

### PATH A — "The Table" (Hotel/Venue)

6. **The Table**
   - The intimate, controlled format. ~100 guests. Luxury venue (Wynn-caliber). Dinner as ritual.
   - The 7 movements: Journey, Arrival, Reception, Dinner, Show, Chill, Journey Back.
   - Emphasis: precision, proximity, sensory control.
   - Shader palette: golds, deep reds, black.
   - Key lines: "Dinner is not intermission. Dinner is ritual." / "Technology amplifies hospitality, it does not replace it."

### PATH B — "The Horizon" (Desert)

7. **The Horizon**
   - The expansive, elemental format. Open landscape. Larger scale.
   - Tribes arriving across terrain. Environmental spectacle.
   - Emphasis: scale, rawness, transformation.
   - Shader palette: blues, silvers, dawn colors.

### ACT III — CONVERGENCE

8. **The Encounter**
   - Both paths reconverge.
   - "Two stages. One civilization. One invitation."
   - Closing: "See you in The Dance... The future isn't coming. It's already moving."

9. **Contact**
   - Minimal form via Formspree. Fields: name, email, message.
   - Clean against black.

10. **The Inner Circle** (gated section)
    - Subtle link/button to password-protected overlay.
    - Client-side JS password check (sufficient for investor context).
    - Behind it: placeholders for full deck PDFs, video embeds, deeper materials per format.

## Technical Stack

| Layer            | Choice                                           |
|------------------|--------------------------------------------------|
| Structure        | Single `index.html`, no build tools              |
| WebGL            | Three.js (particle systems, gradient shaders)    |
| Scroll animation | GSAP ScrollTrigger (CDN)                         |
| Typography       | Google Fonts — elegant serif + clean sans-serif  |
| Contact form     | Formspree (free tier, no backend)                |
| Password gate    | Client-side JS prompt + CSS overlay              |
| Deployment       | Static hosting (Netlify, Vercel, or any server)  |

## Visual Language

- **Palette:** Black base. Hotel gradients: magenta -> orange -> gold. Desert gradients: deep blue -> silver -> dawn.
- **Typography:** Mixed serif/sans echoing the presentation decks. Large, breathing type with generous whitespace.
- **Effects:** Particle fields, shader-driven gradient backgrounds, scroll-driven opacity/transform animations, subtle glow/bloom effects.
- **No images initially** — all generative/code-based. Miro board assets (https://miro.com/app/board/uXjVJ4r3F78=/) to be extracted via Miro API in a separate process and swapped in later.

## Content Sources

- `materiales/presentation1_story_deck/content.md` — primary copy (story deck v1.1)
- `materiales/presentation2/content.md` — secondary copy (design deck v1.0)
- Miro board — visual assets (pending API extraction)

## Out of Scope (for now)

- Miro API image extraction (separate task)
- Real photography/video assets
- Backend/server-side password authentication
- CMS or content management
- Pricing/business model details (kept for in-person meetings)
