# Black Opal Website Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a cinematic, WebGL-powered single-page investor pitch site for Black Opal.

**Architecture:** Single `index.html` with embedded CSS/JS. Three.js for WebGL particle/shader backgrounds. GSAP ScrollTrigger for scroll-driven animations. All libraries loaded via CDN. No build tools.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, flexbox), Three.js (CDN), GSAP + ScrollTrigger (CDN), Google Fonts, Formspree

**Design doc:** `docs/plans/2026-02-25-website-design.md`

**Content sources:** `materiales/presentation1_story_deck/content.md`, `materiales/presentation2/content.md`

---

### Task 1: Scaffold — HTML structure, CDN imports, base styles

**Files:**
- Create: `index.html`

**Step 1: Create the base HTML file**

Set up the complete document skeleton:
- DOCTYPE, html lang="en", head with meta charset, viewport, title
- Google Fonts: load "Playfair Display" (serif, for headlines) and "Inter" (sans, for body)
- CDN links: Three.js (r160+), GSAP core + ScrollTrigger plugin
- A fixed `<canvas id="bg">` for the Three.js WebGL background (full viewport, behind all content)
- 10 empty `<section>` elements with IDs matching the design: `portal`, `reveal`, `vision`, `tribes`, `fork`, `table`, `horizon`, `encounter`, `contact`, `inner-circle`
- Each section: `min-height: 100vh`, flex centering for content
- Base CSS embedded in `<style>`:
  - CSS reset (margin/padding 0, box-sizing border-box)
  - Custom properties: `--black: #0a0a0a`, `--magenta: #c2185b`, `--orange: #e65100`, `--gold: #ffab00`, `--blue-deep: #1a237e`, `--silver: #b0bec5`, `--dawn: #ffccbc`, `--white: #f5f5f5`
  - `html { scroll-behavior: smooth; }` and `body { background: var(--black); color: var(--white); font-family: 'Inter', sans-serif; overflow-x: hidden; }`
  - Canvas: `position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 0; pointer-events: none;`
  - Sections: `position: relative; z-index: 1;`
  - Typography classes: `.headline` (Playfair Display, clamp sizes), `.body-text` (Inter), `.accent` (italic Playfair)

**Step 2: Verify scaffold loads**

Open `index.html` in browser. Confirm: black page, fonts load (check Network tab), no console errors, scrollable through 10 empty sections.

**Step 3: Commit**

```bash
git add index.html
git commit -m "scaffold: base HTML structure with CDN imports and CSS foundation"
```

---

### Task 2: WebGL Background — Three.js particle field

**Files:**
- Modify: `index.html` (add `<script>` block at end of body)

**Step 1: Create the Three.js scene**

Add a `<script>` block before `</body>`. Set up:
- Three.js scene, camera (PerspectiveCamera, fov 75), renderer (targeting the `#bg` canvas, alpha: true, antialias: true)
- A particle system: `BufferGeometry` with ~3000 points randomly distributed in a sphere (radius ~50)
- Each particle: small size (0.08), color starts as white with low opacity
- Use `PointsMaterial` with `vertexColors: true`, `transparent: true`, `opacity: 0.6`, `size: 0.08`, `sizeAttenuation: true`
- Animation loop: gentle rotation of the points group (`rotation.y += 0.0003`, `rotation.x += 0.0001`)
- Resize handler: update camera aspect + renderer size on window resize
- Store scroll progress in a global `scrollProgress` variable (0-1) updated via GSAP ScrollTrigger on the entire page
- In the animation loop, use `scrollProgress` to:
  - Shift particle colors from white → magenta → orange → gold (interpolate via HSL or direct RGB lerp)
  - Slightly change particle spread/density (scale the geometry)

**Step 2: Verify particles render**

Open browser. Confirm: floating particle field visible behind the page. Particles slowly rotate. Scrolling shifts their color. No performance issues (should be 60fps with 3000 particles).

**Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Three.js particle field background with scroll-driven color shifts"
```

---

### Task 3: The Portal — Hero section with logo and tagline

**Files:**
- Modify: `index.html` (populate `#portal` section, add CSS, add GSAP animations)

**Step 1: Build the hero content**

Inside `#portal`:
- A container div centered vertically and horizontally
- The Black Opal logo: recreate using CSS/SVG — a diamond/eye shape above the text. Use an inline SVG: a symmetrical eye-like form with radiating lines (referencing the logo from the decks). White stroke, no fill.
- Below the logo SVG: `<h1>` with "BLACK" on one line and "OPAL" on the next, in Playfair Display, large (clamp(3rem, 8vw, 7rem)), letter-spacing: 0.15em
- Below: `<p class="subtitle">` with "THE DANCE" in Inter, smaller, letter-spacing: 0.3em
- Below: `<p class="tagline accent">` with "An intelligent species has been evolving beside us..."
- Vertical text on left edge: "AN IMMERSIVE HIGH-ART LUXURY EXPERIENCE" (rotated 90deg, fixed position, small Inter text, letter-spacing wide) — this persists through multiple sections
- CSS for portal: center everything, `min-height: 100vh`, padding

**Step 2: Add GSAP entrance animations**

Using GSAP timeline (not scroll-triggered yet — these play on page load):
- Logo SVG draws in (stroke-dashoffset animation, 1.5s)
- "BLACK OPAL" fades up (opacity 0→1, y: 30→0, 1s, staggered per word)
- "THE DANCE" fades in (0.8s delay)
- Tagline fades in last (1.2s delay)
- Vertical side text fades in with the tagline

**Step 3: Verify hero**

Open browser. Confirm: logo draws in, text animates elegantly, centered on viewport, looks premium on both desktop and mobile viewport sizes.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add hero portal section with logo, typography, and entrance animations"
```

---

### Task 4: The Reveal — Scroll-driven text sequence

**Files:**
- Modify: `index.html` (populate `#reveal` section)

**Step 1: Build the reveal content**

Inside `#reveal`:
- Set section height to `300vh` (creates a long scroll zone for the animation)
- A sticky container (`position: sticky; top: 0; height: 100vh`) that stays centered while scrolling
- Three text elements, stacked vertically, each initially `opacity: 0`:
  - `"For one night they invite us inside."`
  - `"What we find changes how we see the future."`
  - `"This is not entertainment you watch."`
- Large, centered Playfair Display text

**Step 2: Add ScrollTrigger animations**

Using GSAP ScrollTrigger pinned to the `#reveal` section:
- As user scrolls through the 300vh zone:
  - First third: line 1 fades in (opacity 0→1, y: 40→0), holds, then fades out
  - Second third: line 2 fades in, holds, fades out
  - Final third: line 3 fades in and stays
- Particle background should be subtly shifting color during this section (handled by global scroll progress from Task 2)

**Step 3: Verify**

Scroll through the reveal section. Text appears one line at a time, smooth transitions. No jank.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add scroll-driven text reveal section"
```

---

### Task 5: The Vision — "What is Black Opal?"

**Files:**
- Modify: `index.html` (populate `#vision` section)

**Step 1: Build the vision content**

Inside `#vision`:
- Section height: `200vh` (scrollable zone)
- Sticky container with centered content, max-width ~800px
- Headline: `<h2 class="headline">WHAT IS BLACK OPAL?</h2>` with a thin horizontal rule below
- Subheadline: "An Immersive Dinner Theater Experience Unlike Anything on Earth."
- Three paragraphs of body text (pulled from content.md slide 5):
  - "Black Opal is a next-generation immersive dinner theater experience designed for a high-net-worth audience..."
  - "Set within a fully controlled environment, robotics and live performers move seamlessly through the space..."
  - Closing italic: "Black Opal is not entertainment you watch. It's entertainment that surrounds you."
- A subtle divider/ornament between paragraphs (thin line or small diamond glyph)

**Step 2: Add scroll animations**

- Headline slides in from left (opacity + translateX)
- Horizontal rule expands from center (scaleX 0→1)
- Each paragraph fades in sequentially as user scrolls (stagger: 0.3)
- Closing italic line gets a subtle glow effect (text-shadow with gold)

**Step 3: Verify**

Scroll to vision section. Content animates in progressively. Typography is readable, layout is balanced.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add vision section with animated prose content"
```

---

### Task 6: The Tribes — Four animated tribe cards

**Files:**
- Modify: `index.html` (populate `#tribes` section)

**Step 1: Build the tribes layout**

Inside `#tribes`:
- Section title: "Four Tribes. Four Expressions of Intelligence."
- A brief intro paragraph: "Each represents a different philosophy of evolution."
- Four tribe cards in a responsive grid (2x2 on desktop, 1-column on mobile)
- Each card contains:
  - A `<canvas>` element (200x200) for the tribe's generative visual
  - Tribe name as heading
  - One-line description
  - Cards have subtle border (1px rgba white), backdrop-filter blur for glass effect

**Tribe visuals (each a small standalone canvas animation):**
- **The Luminous Order**: Radiating concentric circles/lines, white and gold, perfectly symmetrical, slow pulse
- **The Wild Current**: Chaotic colored dots bouncing and swirling, magenta/orange/cyan palette
- **The Reclaimers**: Grid of small squares that glitch and rearrange, green/gray tones
- **The Human Witnesses**: Soft breathing circle, subtle skin-tone warmth, organic pulsing

Implement these as simple 2D canvas (CanvasRenderingContext2D) animations — NOT Three.js. Each tribe canvas gets its own small animation function.

**Step 2: Add scroll entrance animations**

- Section title fades in
- Cards stagger in (opacity + scale, 0.2s stagger) as section enters viewport
- Each tribe canvas animation starts when its card becomes visible

**Step 3: Verify**

Scroll to tribes. Cards animate in with stagger. Each tribe canvas shows its unique generative visual. Grid layout works on different screen sizes.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add tribes section with generative canvas visuals per tribe"
```

---

### Task 7: The Fork — Visual divergence into two paths

**Files:**
- Modify: `index.html` (populate `#fork` section)

**Step 1: Build the fork layout**

Inside `#fork`:
- Section height: `150vh`
- Sticky centered content
- Text: "Two Stages. One Dance." (large Playfair Display, centered)
- Below the text: two visual panels side by side (50/50 split)
  - Left panel: labeled "THE TABLE" — warm gradient overlay (gold → deep red → black), subtle CSS radial gradient simulating interior warmth
  - Right panel: labeled "THE HORIZON" — cool gradient overlay (deep blue → silver → dawn), CSS gradient simulating vast sky/horizon
- A thin vertical dividing line between them (white, 1px, with a diamond ornament at center)
- Each panel has a short teaser line:
  - Left: "The intimate ritual."
  - Right: "The elemental expanse."

**Step 2: Add scroll animations**

- "Two Stages. One Dance." fades in first
- The two panels slide apart from center (start overlapping, separate to 50/50 split)
- Labels and teaser text fade in after panels settle
- The Three.js background particles should split in density (denser on left for warmth, sparser on right for openness) — update the global scroll handler to modify particle distribution in this section's scroll range

**Step 3: Verify**

Scroll to fork. Panels split dramatically. Visual distinction between hotel (warm) and desert (cool) is clear and immediate.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add fork section with dual-path visual divergence"
```

---

### Task 8: The Table — Hotel/venue experience path

**Files:**
- Modify: `index.html` (populate `#table` section)

**Step 1: Build The Table content**

Inside `#table`:
- Section height: `300vh` (long scroll zone for the 7 movements)
- Sticky container, warm color scheme (CSS custom properties shift: borders and accents use gold/warm tones)
- Section header: "THE TABLE" with subtitle "The Intimate Ritual"
- Brief intro paragraph about the hotel format (~100 guests, luxury venue, dinner as the medium)
- The 7 Movements as a vertical timeline:
  - Each movement is a step: THE JOURNEY → ARRIVAL → RECEPTION → DINNER → SHOW → CHILL → JOURNEY BACK
  - Left: movement number/name. Right: 1-2 sentence description
  - Connected by a thin vertical gold line
- Closing quote: "Dinner is not intermission. Dinner is ritual."
- Second quote: "Technology amplifies hospitality, it does not replace it."

**Step 2: Add scroll animations**

- Section header fades in with warm glow (gold text-shadow)
- Timeline steps reveal one by one as user scrolls (each step: opacity + translateY, with the connecting line growing downward)
- Closing quotes fade in with emphasis (slight scale + glow)
- Background particles shift to warm palette (gold/amber) during this section

**Step 3: Verify**

Scroll through The Table. Timeline unfolds progressively. Warm color palette feels distinct from the neutral shared sections. Quotes land with impact.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add The Table section with 7-movement timeline"
```

---

### Task 9: The Horizon — Desert experience path

**Files:**
- Modify: `index.html` (populate `#horizon` section)

**Step 1: Build The Horizon content**

Inside `#horizon`:
- Section height: `250vh`
- Sticky container, cool color scheme (accents shift to silver/blue/dawn tones)
- Section header: "THE HORIZON" with subtitle "The Elemental Expanse"
- Brief intro: the expansive format, open landscape, tribes arriving across terrain, environmental spectacle at scale
- Key themes presented as large statement blocks (not a timeline — this format is more raw/open):
  - "Scale" — "Where the landscape becomes the stage."
  - "Encounter" — "Tribes emerge from the horizon. You see them before you hear them."
  - "Transformation" — "Under open sky, the boundaries dissolve entirely."
- Each theme: full-width text block with the keyword large and description below
- Closing line: "No walls. No ceiling. No separation."

**Step 2: Add scroll animations**

- Section header fades in with cool glow (blue/silver text-shadow)
- Theme blocks slide in from alternating sides (left, right, left) as user scrolls
- Background particles shift to cool palette (blue/silver/white) during this section
- Particles become more spread out (increase geometry scale) to evoke vastness

**Step 3: Verify**

Scroll through The Horizon. Cool palette feels distinct from The Table. Theme blocks animate with weight. Particle field opens up visually.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add The Horizon section with desert experience themes"
```

---

### Task 10: The Encounter — Convergence and closing

**Files:**
- Modify: `index.html` (populate `#encounter` section)

**Step 1: Build convergence content**

Inside `#encounter`:
- Section height: `200vh`
- Sticky centered content
- Line 1: "Two stages." (medium text)
- Line 2: "One civilization." (larger)
- Line 3: "One invitation." (largest, with subtle gradient text effect: magenta → gold via `background-clip: text`)
- Pause/spacer
- Closing statement in accent italic Playfair: "See you in The Dance..."
- Subtitle: "The future isn't coming. It's already moving."

**Step 2: Add scroll animations**

- Lines 1-3 fade in sequentially with increasing size/weight
- "One invitation" gets the gradient text treatment with a subtle shimmer animation (background-position shift)
- Closing statement fades in last with a slow, deliberate timing
- Particles return to the full-spectrum gradient (magenta → gold) and gently converge toward center

**Step 3: Verify**

Scroll through encounter. The build-up feels climactic. Gradient text is visible and renders correctly. Emotional landing of the closing statement.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add convergence section with closing statement"
```

---

### Task 11: Contact Form — Formspree integration

**Files:**
- Modify: `index.html` (populate `#contact` section)

**Step 1: Build the contact form**

Inside `#contact`:
- Minimal layout, centered, max-width 500px
- Small heading: "Get in Touch" or "Request a Meeting"
- `<form>` with `action="https://formspree.io/f/{FORM_ID}"` method="POST"
  - Use a placeholder form ID for now: `xExample123` — user will replace with their real Formspree endpoint
  - Fields: Name (text input), Email (email input), Message (textarea, 4 rows)
  - Submit button: "Send" — styled minimally (white border, transparent bg, white text, hover: white bg + black text)
- All inputs: transparent background, white border-bottom only (no full border), white text, Playfair placeholder text
- A comment in the HTML: `<!-- Replace xExample123 with your Formspree form ID -->`

**Step 2: Add subtle animations**

- Form fades in on scroll entry (simple opacity + translateY)
- Input focus states: border-bottom transitions to gold

**Step 3: Verify**

Scroll to contact. Form renders cleanly. Inputs are usable, focus states work. Submit doesn't crash (will 404 with placeholder ID, that's expected).

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add contact form section with Formspree placeholder"
```

---

### Task 12: The Inner Circle — Password-gated section

**Files:**
- Modify: `index.html` (populate `#inner-circle` section)

**Step 1: Build the gate and hidden content**

Inside `#inner-circle`:
- A subtle centered link/button: "Enter The Inner Circle" — small, understated, with a thin border
- Clicking it reveals a full-screen overlay (`position: fixed`, `z-index: 100`, dark background with backdrop-filter blur)
- Overlay contains:
  - A password input (styled same as contact form inputs) with placeholder "Access Code"
  - A submit button: "Enter"
  - A close/X button in the top-right corner
- Password check: simple JS comparison against a hardcoded value (default: `"blackopal2026"`)
  - On correct password: overlay content swaps to the inner circle materials
  - On wrong password: subtle red flash on the input, text "Invalid access code"
- Inner circle content (shown after correct password):
  - Heading: "The Inner Circle"
  - Two cards/sections:
    - "The Table — Full Deck" with a placeholder download link (`<a href="#">Download PDF</a>`)
    - "The Horizon — Full Deck" with a placeholder download link
  - A placeholder video embed area: "Video coming soon" with a bordered rectangle
  - A "Close" button to dismiss the overlay

**Step 2: Verify**

Click "Enter The Inner Circle". Overlay appears with blur. Wrong password shows error. Correct password ("blackopal2026") reveals placeholder content. Close button works.

**Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add password-gated inner circle section with placeholder content"
```

---

### Task 13: Polish — Responsive design, performance, final touches

**Files:**
- Modify: `index.html`

**Step 1: Responsive breakpoints**

Add media queries:
- `max-width: 768px`:
  - Tribe cards: single column
  - Fork panels: stack vertically instead of side-by-side
  - Reduce heading font sizes (clamp values should handle most of this)
  - Vertical side text: hide on mobile (too small to read)
  - Reduce particle count to ~1500 for mobile performance
- `max-width: 480px`:
  - Further font size reductions
  - Increase section padding for thumb-friendly spacing

**Step 2: Performance optimizations**

- Add `will-change: transform, opacity` to animated elements
- Add `loading="lazy"` to any future image placeholders
- Ensure Three.js renderer uses `powerPreference: 'high-performance'`
- Add a `devicePixelRatio` cap (`Math.min(window.devicePixelRatio, 2)`) to prevent 3x/4x rendering on high-DPI screens
- Reduce particle count if `navigator.hardwareConcurrency < 4`

**Step 3: Scroll smoothness**

- Ensure GSAP ScrollTrigger has `smoothChildTiming: true`
- Add `scrub: 1` (1-second smoothing) to all scroll-triggered animations for buttery feel
- Test that all pinned/sticky sections release cleanly

**Step 4: Accessibility basics**

- Add `prefers-reduced-motion` media query: disable particle animation, disable scroll-driven transforms, show all text immediately
- Ensure form inputs have proper `<label>` elements (can be visually hidden)
- Add `aria-label` to the inner circle overlay
- Ensure sufficient color contrast for body text (white on black is fine)

**Step 5: Final visual polish**

- Add a subtle CSS noise texture overlay on top of everything (`background-image` with a tiny noise PNG data URI, very low opacity ~0.03) for a film-grain feel
- Ensure the Black Opal logo SVG in the hero matches the eye/diamond motif from the decks
- Add a favicon: small inline SVG data URI of the logo shape
- Add `<meta>` tags: description, og:title, og:description for sharing

**Step 6: Verify full experience**

Open in browser. Scroll through the entire site top to bottom. Check:
- All sections animate correctly
- Particle background shifts color through the journey
- Fork section clearly distinguishes the two paths
- Contact form is functional
- Inner circle gate works
- Mobile layout is usable (test at 375px and 768px)
- No console errors
- Performance: smooth 60fps scroll

**Step 7: Commit**

```bash
git add index.html
git commit -m "polish: responsive design, performance, accessibility, and visual refinements"
```

---

## Summary

| Task | Section | Description |
|------|---------|-------------|
| 1 | Scaffold | HTML structure, CDN imports, base CSS |
| 2 | Background | Three.js particle field with scroll-driven color |
| 3 | Portal | Hero with logo, title, entrance animations |
| 4 | Reveal | Scroll-driven sequential text reveals |
| 5 | Vision | "What is Black Opal?" cinematic prose |
| 6 | Tribes | Four tribe cards with generative canvas visuals |
| 7 | Fork | Visual divergence into hotel/desert paths |
| 8 | Table | Hotel experience — 7-movement timeline |
| 9 | Horizon | Desert experience — thematic statement blocks |
| 10 | Encounter | Convergence and closing statement |
| 11 | Contact | Formspree contact form |
| 12 | Inner Circle | Password-gated section for deeper materials |
| 13 | Polish | Responsive, performance, accessibility, final touches |
