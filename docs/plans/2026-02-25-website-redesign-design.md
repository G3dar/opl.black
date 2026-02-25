# OPL.BLACK Website Redesign - Immersive Cinematic Scroll

## Date: 2026-02-25

## Summary

Complete redesign of opl.black from a text-heavy narrative site to a cinematic scroll gallery centered on robot imagery from the Black Opal deck. Minimal text, maximum visual impact, mystical luxury vibe.

## Approach: Scroll Cinematografico

A linear film-like experience where each scroll reveals a new scene. Robots appear full-bleed with parallax, interleaved with short phrases from the deck floating over sunset gradients. No visible section boundaries - everything flows as a continuous sequence.

## Structure (5 scenes)

### 1. Portal (100vh)
- Animated sunset gradient background (colors shift subtly, matching slide 1/40 of deck)
- Eye/diamond SVG logo center, stroke animation on entry
- "BLACK OPAL" in large modern typography
- Three.js particles (reduced count, sunset palette colors)
- Subtle scroll indicator at bottom

### 2. Awakening (150vh, sticky scroll)
- "An intelligent species has been evolving beside us..." appears word-by-word on scroll
- Phrase dissolves, first robot image (two robots in desert) scales up from center
- Transition: sunset gradient darkens to black as robot appears

### 3. The Tribes (~300vh, longest section)
- Sequence of full-bleed robot images with parallax
- Between every 2-3 images, a short deck phrase:
  - "For one night they invite us inside"
  - "Four tribes. Four expressions of intelligence."
  - "What we find changes how we see the future."
- Images alternate layouts: full-width, split (two side-by-side), centered close-up
- Subtle sunset gradient overlay on each image to unify palette
- Transitions: crossfade or vertical slide via GSAP ScrollTrigger

### 4. The Dance (100vh)
- Neon light installation image (image_0034) as full-bleed background
- "THE DANCE" in enormous typography with blue/magenta glow effect
- "The future isn't coming. It's already moving." below
- Subtle parallax on image, gentle text pulse

### 5. The Invitation (50vh)
- Inverted sunset gradient (bottom-up: orange > magenta > black)
- "SEE YOU IN THE DANCE..." centered
- Minimal contact: email + message only (Formspree integration preserved)
- Inner circle password gate preserved at very end

## Typography

- **Headings/phrases**: Syne (geometric, futuristic yet elegant) or Space Grotesk
- **Body (minimal)**: Inter (kept from current site)
- Sizes: Large and bold, using clamp() for fluid responsiveness

## Color Palette

- Base: `#0a0a0a` (black, kept)
- Sunset gradient: `#1a0a15` > `#8b1a4a` > `#c2185b` > `#e65100` > `#ff8f00`
- Neon accent: `#00d4ff`
- Text: `#f5f5f5` with variable opacity

## Images

- 8-12 hero images selected from Miro board downloads + deck slides
- Optimized to WebP format
- Lazy loaded with blur-up placeholder
- Sources: miro_board_fresh/, miro_board/section_01_top/, presentation slides

## Technical Stack

- Single HTML file (current approach maintained)
- Three.js for particle background (sunset colors, reduced count)
- GSAP + ScrollTrigger for all scroll animations
- Native lazy loading for images
- WebP with PNG fallback

## What's Removed

- All heavy text sections (Vision, Table timeline, Horizon, pricing, venue, partners, emotional arc)
- Tribe canvas generative animations (replaced by actual robot images)
- Fork section (two paths concept)
- Complex multi-step narrative structure

## What's Preserved

- Contact form (simplified to email + message)
- Inner circle password gate (password: blackopal2026)
- Three.js particle background (adapted to sunset palette)
- GSAP/ScrollTrigger animation framework
- Responsive design approach
- CNAME for opl.black domain
