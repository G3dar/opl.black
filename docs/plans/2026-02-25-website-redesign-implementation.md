# OPL.BLACK Cinematic Scroll Redesign - Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Replace the text-heavy opl.black site with a cinematic scroll gallery centered on robot imagery, sunset gradients, and minimal text.

**Architecture:** Single-file HTML rewrite (index.html, ~1983 lines currently). Keep Three.js + GSAP + ScrollTrigger stack. Replace all 10 content sections with 5 visual scenes. Add actual robot images from materiales/ as WebP assets served from an `assets/` folder.

**Tech Stack:** HTML/CSS/JS (single file), Three.js 0.160, GSAP 3.12.5 + ScrollTrigger, Google Fonts (Syne + Inter), WebP images with lazy loading.

---

### Task 1: Prepare Image Assets

Select and optimize the best robot images for the site. Copy them to a public `assets/` folder.

**Files:**
- Create: `assets/robots/` directory
- Create: `assets/bg/` directory
- Source: `materiales/miro_board_fresh/`, `materiales/miro_board/section_01_top/`, `materiales/presentation1_story_deck/`

**Step 1: Create asset directories**

```bash
mkdir -p assets/robots assets/bg
```

**Step 2: Select and copy best images**

Robot images for The Tribes gallery (pick ~8-10 of the best):
```bash
# Desert robots - groups and close-ups
cp materiales/miro_board_fresh/image_0010.png assets/robots/tribes-desert-duo.png
cp materiales/miro_board_fresh/image_0012.png assets/robots/tribes-fashion-lineup.png
cp materiales/miro_board_fresh/image_0015.png assets/robots/tribes-floral-group.png
cp materiales/miro_board_fresh/image_0020.png assets/robots/tribes-opal-closeup.png
cp materiales/miro_board_fresh/image_0025.png assets/robots/tribes-masked-four.png
cp materiales/miro_board_fresh/image_0003.png assets/robots/tribes-creature.png
cp materiales/miro_board_fresh/image_0018.png assets/robots/tribes-wide-group.png
cp materiales/miro_board_fresh/image_0028.png assets/robots/tribes-detail.png
```

Atmospheric/background images:
```bash
cp materiales/miro_board_fresh/image_0005.png assets/bg/mountain-halo.png
cp materiales/miro_board_fresh/image_0030.png assets/bg/neon-landscape.png
cp materiales/miro_board_fresh/image_0034.png assets/bg/neon-cave.png
```

Logo reference:
```bash
cp materiales/miro_board/section_01_top/image_008.png assets/bg/sunset-logo.png
```

**Step 3: Convert to WebP for performance**

```bash
# If cwebp available:
for f in assets/robots/*.png assets/bg/*.png; do
  cwebp -q 85 "$f" -o "${f%.png}.webp" 2>/dev/null && echo "Converted $f"
done
# If cwebp not available, keep PNGs - they'll work fine, just larger
```

**Step 4: Verify all images exist and are valid (>10KB each)**

```bash
find assets/ -type f \( -name "*.webp" -o -name "*.png" \) -exec ls -lh {} \;
```

**Step 5: Commit**

```bash
git add assets/
git commit -m "assets: add selected robot and atmosphere images for redesign"
```

---

### Task 2: Rewrite HTML Structure

Replace all 10 sections in index.html body with the 5 new scenes. Keep the canvas, inner circle overlay, and meta tags.

**Files:**
- Modify: `index.html` (lines 1170-1387, the `<body>` content)

**Step 1: Replace the font import**

Change line 18 from Playfair Display to Syne:
```html
<!-- Old -->
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">

<!-- New -->
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
```

**Step 2: Replace body HTML (between `<body>` and `<script>`)**

Replace everything from line 1170 (`<body>`) through line 1387 (before `<script>`) with:

```html
<body>

  <canvas id="bg"></canvas>

  <div class="vertical-text">AN IMMERSIVE HIGH-ART LUXURY EXPERIENCE</div>

  <!-- Scene 1: Portal -->
  <section id="portal">
    <div class="portal-container">
      <svg class="logo-svg" viewBox="0 0 120 100" xmlns="http://www.w3.org/2000/svg">
        <path d="M10,50 Q60,5 110,50 Q60,95 10,50 Z"/>
        <path d="M60,25 L80,50 L60,75 L40,50 Z"/>
        <line x1="60" y1="10" x2="60" y2="90"/>
        <line x1="20" y1="50" x2="100" y2="50"/>
        <circle cx="60" cy="50" r="8"/>
      </svg>
      <h1 class="portal-title">
        <span>BLACK</span>
        <span>OPAL</span>
      </h1>
      <div class="scroll-hint">
        <div class="scroll-line"></div>
      </div>
    </div>
    <div class="portal-gradient"></div>
  </section>

  <!-- Scene 2: Awakening -->
  <section id="awakening">
    <div class="awakening-sticky">
      <p class="awakening-text" id="awakening-text">An intelligent species has been evolving beside us...</p>
      <div class="awakening-image" id="awakening-image">
        <img src="assets/robots/tribes-desert-duo.png" alt="Two robotic figures in desert landscape" loading="lazy">
      </div>
    </div>
  </section>

  <!-- Scene 3: The Tribes -->
  <section id="tribes">
    <div class="tribes-flow">

      <div class="tribe-frame full" data-speed="0.8">
        <img src="assets/robots/tribes-fashion-lineup.png" alt="Robotic figures in ornate fashion" loading="lazy">
      </div>

      <div class="tribe-phrase">
        <p>...For one night they invite us inside.</p>
      </div>

      <div class="tribe-frame split">
        <img src="assets/robots/tribes-floral-group.png" alt="Floral-adorned robotic tribe" loading="lazy" class="split-left">
        <img src="assets/robots/tribes-opal-closeup.png" alt="Close-up of opalescent robots" loading="lazy" class="split-right">
      </div>

      <div class="tribe-phrase">
        <p>Four tribes. Four expressions of intelligence.</p>
      </div>

      <div class="tribe-frame full" data-speed="1.2">
        <img src="assets/robots/tribes-masked-four.png" alt="Four masked robotic figures" loading="lazy">
      </div>

      <div class="tribe-phrase">
        <p>What we find changes how we see the future.</p>
      </div>

      <div class="tribe-frame full" data-speed="0.9">
        <img src="assets/robots/tribes-creature.png" alt="Hybrid creature figure" loading="lazy">
      </div>

      <div class="tribe-frame split">
        <img src="assets/robots/tribes-wide-group.png" alt="Wide shot of robotic tribe" loading="lazy" class="split-left">
        <img src="assets/robots/tribes-detail.png" alt="Detailed robot craftsmanship" loading="lazy" class="split-right">
      </div>

    </div>
  </section>

  <!-- Scene 4: The Dance -->
  <section id="dance">
    <div class="dance-bg">
      <img src="assets/bg/neon-cave.png" alt="Immersive neon light installation" loading="lazy">
    </div>
    <div class="dance-content">
      <h2 class="dance-title">THE DANCE</h2>
      <p class="dance-subtitle">The future isn't coming. It's already moving.</p>
    </div>
  </section>

  <!-- Scene 5: The Invitation -->
  <section id="invitation">
    <div class="invitation-content">
      <p class="invitation-phrase">SEE YOU IN THE DANCE...</p>
      <form class="invitation-form" action="https://formspree.io/f/xExample123" method="POST">
        <input type="email" name="email" placeholder="Your email" required>
        <textarea name="message" rows="2" placeholder="Message (optional)"></textarea>
        <button type="submit">Request Access</button>
      </form>
    </div>
  </section>

  <!-- Inner Circle (preserved) -->
  <section id="inner-circle">
    <button class="inner-circle-trigger" id="ic-trigger">Enter The Inner Circle</button>
  </section>

  <!-- Inner Circle Overlay (preserved) -->
  <div class="ic-overlay" id="ic-overlay" aria-label="The Inner Circle">
    <button class="ic-close" id="ic-close-x" aria-label="Close">&times;</button>
    <div class="ic-modal">
      <h2 class="headline" style="margin-bottom:1.5rem; font-size:1.8rem;">The Inner Circle</h2>
      <div class="ic-gate" id="ic-gate">
        <input type="password" id="ic-password" placeholder="Access Code" autocomplete="off">
        <p class="ic-error-msg" id="ic-error"></p>
        <button id="ic-submit">Enter</button>
      </div>
      <div class="ic-content" id="ic-content">
        <div class="ic-cards">
          <div class="ic-card">
            <h3>The Table — Full Deck</h3>
            <p>Download the complete production document for The Table experience.</p>
            <a href="#">Download PDF</a>
          </div>
          <div class="ic-card">
            <h3>The Horizon — Full Deck</h3>
            <p>Download the complete production document for The Horizon experience.</p>
            <a href="#">Download PDF</a>
          </div>
        </div>
        <div class="ic-video-placeholder">
          <p>Video content coming soon.</p>
        </div>
        <button class="ic-close-btn" id="ic-close-btn">Close</button>
      </div>
    </div>
  </div>
```

**Step 3: Verify HTML is valid (no unclosed tags)**

Open index.html in browser, check console for errors.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: replace HTML structure with 5-scene cinematic layout"
```

---

### Task 3: Rewrite CSS

Replace all CSS (lines 27-1167 in `<style>`) with new styles for the 5 scenes.

**Files:**
- Modify: `index.html` (the `<style>` block, lines 27-1167)

**Step 1: Replace the entire `<style>` block**

Replace everything inside `<style>...</style>` with:

```css
/* === Reset === */
*, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

/* === Custom Properties === */
:root {
  --black: #0a0a0a;
  --magenta: #c2185b;
  --magenta-dark: #8b1a4a;
  --orange: #e65100;
  --gold: #ff8f00;
  --neon: #00d4ff;
  --white: #f5f5f5;
  --sunset-1: #1a0a15;
  --sunset-2: #8b1a4a;
  --sunset-3: #c2185b;
  --sunset-4: #e65100;
  --sunset-5: #ff8f00;
}

html { scroll-behavior: smooth; }

body {
  background: var(--black);
  color: var(--white);
  font-family: 'Inter', sans-serif;
  overflow-x: hidden;
  line-height: 1.6;
}

/* === WebGL Canvas === */
#bg {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  z-index: 0;
  pointer-events: none;
}

/* === Noise Overlay === */
body::after {
  content: '';
  position: fixed;
  inset: 0;
  z-index: 999;
  pointer-events: none;
  opacity: 0.03;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}

/* === Vertical Text === */
.vertical-text {
  position: fixed;
  left: 1.5rem;
  top: 50%;
  transform: rotate(-90deg) translateX(-50%);
  transform-origin: left center;
  font-size: 0.6rem;
  font-weight: 300;
  letter-spacing: 0.35em;
  text-transform: uppercase;
  color: rgba(245,245,245,0.3);
  white-space: nowrap;
  z-index: 10;
  opacity: 0;
}

/* === Sections Base === */
section {
  position: relative;
  z-index: 1;
}

/* ==================== */
/* SCENE 1: PORTAL      */
/* ==================== */
#portal {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.portal-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
  z-index: 2;
}

.portal-gradient {
  position: absolute;
  inset: 0;
  z-index: 1;
  background: linear-gradient(
    180deg,
    #0a0a0a 0%,
    #1a0a15 15%,
    #4a1942 30%,
    #8b1a4a 45%,
    #c2185b 55%,
    #e65100 70%,
    #ff8f00 85%,
    #e65100 95%,
    #1a0a15 100%
  );
  opacity: 0.6;
}

.logo-svg {
  width: clamp(80px, 12vw, 140px);
  height: auto;
  opacity: 0;
  z-index: 2;
}

.logo-svg path, .logo-svg line, .logo-svg circle {
  stroke: var(--white);
  stroke-width: 1.5;
  fill: none;
  stroke-dasharray: 500;
  stroke-dashoffset: 500;
}

.portal-title {
  font-family: 'Syne', sans-serif;
  font-size: clamp(3.5rem, 10vw, 9rem);
  font-weight: 800;
  letter-spacing: 0.08em;
  line-height: 0.95;
  opacity: 0;
  text-align: center;
  z-index: 2;
}

.portal-title span { display: block; }

.scroll-hint {
  margin-top: 3rem;
  opacity: 0;
  z-index: 2;
}

.scroll-line {
  width: 1px;
  height: 60px;
  background: linear-gradient(to bottom, var(--white), transparent);
  animation: scrollPulse 2s ease-in-out infinite;
}

@keyframes scrollPulse {
  0%, 100% { opacity: 0.3; transform: scaleY(0.6); }
  50% { opacity: 1; transform: scaleY(1); }
}

/* ==================== */
/* SCENE 2: AWAKENING   */
/* ==================== */
#awakening {
  height: 200vh;
  min-height: auto;
}

.awakening-sticky {
  position: sticky;
  top: 0;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.awakening-text {
  font-family: 'Syne', sans-serif;
  font-size: clamp(1.6rem, 4vw, 3.5rem);
  font-weight: 600;
  text-align: center;
  max-width: 800px;
  padding: 0 2rem;
  line-height: 1.3;
  z-index: 2;
}

.awakening-image {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transform: scale(0.8);
  z-index: 1;
}

.awakening-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* ==================== */
/* SCENE 3: THE TRIBES  */
/* ==================== */
#tribes {
  display: flex;
  flex-direction: column;
  padding: 0;
}

.tribes-flow {
  display: flex;
  flex-direction: column;
  gap: 0;
}

.tribe-frame {
  width: 100%;
  overflow: hidden;
  opacity: 0;
  transform: translateY(60px);
}

.tribe-frame.full {
  height: 100vh;
}

.tribe-frame.full img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.tribe-frame.split {
  height: 80vh;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4px;
}

.tribe-frame.split img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.tribe-phrase {
  min-height: 60vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4rem 2rem;
}

.tribe-phrase p {
  font-family: 'Syne', sans-serif;
  font-size: clamp(1.4rem, 3.5vw, 3rem);
  font-weight: 600;
  text-align: center;
  max-width: 700px;
  opacity: 0;
  background: linear-gradient(90deg, var(--magenta), var(--orange), var(--gold));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* ==================== */
/* SCENE 4: THE DANCE   */
/* ==================== */
#dance {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.dance-bg {
  position: absolute;
  inset: 0;
  z-index: 1;
}

.dance-bg img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.7;
}

.dance-content {
  z-index: 2;
  text-align: center;
  opacity: 0;
}

.dance-title {
  font-family: 'Syne', sans-serif;
  font-size: clamp(4rem, 12vw, 10rem);
  font-weight: 800;
  letter-spacing: 0.05em;
  line-height: 1;
  text-shadow: 0 0 80px rgba(0,212,255,0.4), 0 0 160px rgba(194,24,91,0.3);
}

.dance-subtitle {
  font-family: 'Inter', sans-serif;
  font-size: clamp(0.9rem, 1.5vw, 1.2rem);
  font-weight: 300;
  letter-spacing: 0.1em;
  margin-top: 1.5rem;
  opacity: 0.8;
}

/* ======================== */
/* SCENE 5: THE INVITATION  */
/* ======================== */
#invitation {
  min-height: 60vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(0deg, var(--sunset-5) 0%, var(--sunset-4) 15%, var(--sunset-3) 35%, var(--sunset-2) 55%, var(--sunset-1) 80%, var(--black) 100%);
}

.invitation-content {
  text-align: center;
  padding: 4rem 2rem;
  opacity: 0;
}

.invitation-phrase {
  font-family: 'Syne', sans-serif;
  font-size: clamp(2rem, 5vw, 4rem);
  font-weight: 700;
  margin-bottom: 3rem;
}

.invitation-form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  max-width: 400px;
  margin: 0 auto;
}

.invitation-form input,
.invitation-form textarea {
  background: rgba(255,255,255,0.08);
  border: 1px solid rgba(255,255,255,0.15);
  border-radius: 4px;
  padding: 0.9rem 1.2rem;
  color: var(--white);
  font-family: 'Inter', sans-serif;
  font-size: 0.95rem;
  transition: border-color 0.3s;
}

.invitation-form input:focus,
.invitation-form textarea:focus {
  outline: none;
  border-color: var(--magenta);
}

.invitation-form button {
  background: transparent;
  border: 1px solid var(--white);
  color: var(--white);
  padding: 0.9rem 2rem;
  font-family: 'Syne', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  cursor: pointer;
  border-radius: 4px;
  transition: all 0.3s;
}

.invitation-form button:hover {
  background: var(--white);
  color: var(--black);
}

/* ======================== */
/* INNER CIRCLE (preserved) */
/* ======================== */
#inner-circle {
  min-height: 30vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.inner-circle-trigger {
  background: none;
  border: 1px solid rgba(245,245,245,0.2);
  color: rgba(245,245,245,0.5);
  padding: 0.8rem 2rem;
  font-family: 'Syne', sans-serif;
  font-size: 0.75rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.3s;
}

.inner-circle-trigger:hover {
  border-color: var(--white);
  color: var(--white);
}

.ic-overlay {
  position: fixed;
  inset: 0;
  background: rgba(10,10,10,0.95);
  backdrop-filter: blur(16px);
  z-index: 100;
  display: none;
  align-items: center;
  justify-content: center;
}

.ic-overlay.active { display: flex; }

.ic-modal {
  max-width: 600px;
  width: 90%;
  text-align: center;
  padding: 3rem 2rem;
}

.ic-close {
  position: absolute;
  top: 2rem;
  right: 2rem;
  background: none;
  border: none;
  color: var(--white);
  font-size: 2rem;
  cursor: pointer;
}

.ic-gate { display: flex; flex-direction: column; gap: 1rem; }

.ic-gate input {
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.15);
  border-radius: 4px;
  padding: 1rem;
  color: var(--white);
  font-family: 'Inter', sans-serif;
  font-size: 1rem;
  text-align: center;
  letter-spacing: 0.2em;
}

.ic-gate input:focus { outline: none; border-color: var(--magenta); }
.ic-error-msg { color: var(--magenta); font-size: 0.85rem; min-height: 1.2em; }

.ic-gate button,
.ic-close-btn {
  background: none;
  border: 1px solid var(--white);
  color: var(--white);
  padding: 0.8rem 2rem;
  font-family: 'Syne', sans-serif;
  font-size: 0.85rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.3s;
}

.ic-gate button:hover,
.ic-close-btn:hover {
  background: var(--white);
  color: var(--black);
}

.ic-content { display: none; }
.ic-content.visible { display: block; }

.ic-cards {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  margin: 2rem 0;
}

.ic-card {
  background: rgba(255,255,255,0.03);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 8px;
  padding: 1.5rem;
  text-align: left;
}

.ic-card h3 {
  font-family: 'Syne', sans-serif;
  font-size: 1rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.ic-card p {
  font-size: 0.85rem;
  opacity: 0.7;
  margin-bottom: 1rem;
}

.ic-card a {
  color: var(--neon);
  font-size: 0.85rem;
  text-decoration: none;
}

.ic-video-placeholder {
  padding: 2rem;
  opacity: 0.5;
  font-size: 0.9rem;
}

.headline {
  font-family: 'Syne', sans-serif;
  font-weight: 700;
}

/* ============ */
/* RESPONSIVE   */
/* ============ */
@media (max-width: 768px) {
  .tribe-frame.split {
    grid-template-columns: 1fr;
    height: auto;
  }
  .tribe-frame.split img {
    height: 50vh;
  }
  .ic-cards {
    grid-template-columns: 1fr;
  }
  .vertical-text { display: none; }
}

@media (max-width: 480px) {
  .portal-title {
    letter-spacing: 0.04em;
  }
  .tribe-phrase {
    min-height: 40vh;
    padding: 3rem 1.5rem;
  }
}

/* === Reduced Motion === */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
  .scroll-line { animation: none; opacity: 0.5; }
}
```

**Step 2: Verify styles render correctly**

Open in browser. Should see: sunset gradient portal, scroll sections, dark backgrounds.

**Step 3: Commit**

```bash
git add index.html
git commit -m "feat: replace CSS with cinematic scroll styles and sunset palette"
```

---

### Task 4: Rewrite JavaScript - Particles & Portal Animation

Replace all JS (lines 1388-1980) with new animations. This task covers the Three.js particles and portal entrance animation.

**Files:**
- Modify: `index.html` (the `<script>` block)

**Step 1: Replace the entire `<script>...</script>` block**

Write the Three.js particle system (sunset palette) and portal entrance animation:

```javascript
/* === THREE.JS PARTICLES === */
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 1000);
camera.position.z = 50;

const renderer = new THREE.WebGLRenderer({
  canvas: document.getElementById('bg'),
  alpha: true,
  antialias: false
});
renderer.setSize(innerWidth, innerHeight);
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));

const isMobile = innerWidth < 768;
const particleCount = isMobile ? 800 : 2000;
const geo = new THREE.BufferGeometry();
const positions = new Float32Array(particleCount * 3);
const colors = new Float32Array(particleCount * 3);

// Sunset palette for particles
const sunsetColors = [
  new THREE.Color('#f5f5f5'),
  new THREE.Color('#c2185b'),
  new THREE.Color('#e65100'),
  new THREE.Color('#ff8f00'),
  new THREE.Color('#00d4ff')
];

for (let i = 0; i < particleCount; i++) {
  const theta = Math.random() * Math.PI * 2;
  const phi = Math.acos(2 * Math.random() - 1);
  const r = 30 + Math.random() * 40;
  positions[i*3]     = r * Math.sin(phi) * Math.cos(theta);
  positions[i*3 + 1] = r * Math.sin(phi) * Math.sin(theta);
  positions[i*3 + 2] = r * Math.cos(phi);

  const c = sunsetColors[Math.floor(Math.random() * sunsetColors.length)];
  colors[i*3]     = c.r;
  colors[i*3 + 1] = c.g;
  colors[i*3 + 2] = c.b;
}

geo.setAttribute('position', new THREE.BufferAttribute(positions, 3));
geo.setAttribute('color', new THREE.BufferAttribute(colors, 3));

const mat = new THREE.PointsMaterial({
  size: isMobile ? 0.15 : 0.1,
  vertexColors: true,
  transparent: true,
  opacity: 0.6,
  sizeAttenuation: true
});

const particles = new THREE.Points(geo, mat);
scene.add(particles);

let scrollY = 0;
window.addEventListener('scroll', () => { scrollY = window.scrollY; }, { passive: true });

function animate() {
  requestAnimationFrame(animate);
  const t = Date.now() * 0.0001;
  particles.rotation.y = t + scrollY * 0.0001;
  particles.rotation.x = Math.sin(t) * 0.1;
  // Fade particles based on scroll
  const scrollFade = Math.max(0, 1 - scrollY / (innerHeight * 3));
  mat.opacity = 0.6 * scrollFade;
  renderer.render(scene, camera);
}
animate();

window.addEventListener('resize', () => {
  camera.aspect = innerWidth / innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(innerWidth, innerHeight);
});

/* === GSAP ANIMATIONS === */
gsap.registerPlugin(ScrollTrigger);

// Portal entrance
const portalTl = gsap.timeline({ defaults: { ease: 'power3.out' } });
portalTl
  .to('.logo-svg', { opacity: 1, duration: 0.5 })
  .to('.logo-svg path, .logo-svg line, .logo-svg circle', {
    strokeDashoffset: 0,
    duration: 1.5,
    stagger: 0.15,
    ease: 'power2.inOut'
  }, '<')
  .to('.portal-title', {
    opacity: 1,
    y: 0,
    duration: 1,
  }, '-=0.5')
  .from('.portal-title span', {
    y: 40,
    opacity: 0,
    stagger: 0.2,
    duration: 0.8
  }, '<')
  .to('.scroll-hint', { opacity: 1, duration: 0.8 }, '-=0.3')
  .to('.vertical-text', { opacity: 0.4, duration: 0.8 }, '<');
```

Continue in next step (Task 5) with scroll animations.

**Step 2: Commit**

```bash
git add index.html
git commit -m "feat: add Three.js sunset particles and portal entrance animation"
```

---

### Task 5: Rewrite JavaScript - Scroll Animations

Add all GSAP ScrollTrigger animations for scenes 2-5 and the inner circle gate.

**Files:**
- Modify: `index.html` (append to the `<script>` block, after Task 4's code)

**Step 1: Add Awakening scroll animation**

```javascript
/* === SCENE 2: AWAKENING === */
const awakeningTl = gsap.timeline({
  scrollTrigger: {
    trigger: '#awakening',
    start: 'top top',
    end: 'bottom bottom',
    scrub: 1,
    pin: '.awakening-sticky'
  }
});

awakeningTl
  .fromTo('#awakening-text',
    { opacity: 1 },
    { opacity: 0, scale: 0.9, duration: 0.5 }
  )
  .fromTo('#awakening-image',
    { opacity: 0, scale: 0.8 },
    { opacity: 1, scale: 1, duration: 0.5 },
    0.3
  );
```

**Step 2: Add Tribes scroll animations**

```javascript
/* === SCENE 3: THE TRIBES === */
// Each frame fades in on scroll
gsap.utils.toArray('.tribe-frame').forEach(frame => {
  gsap.to(frame, {
    opacity: 1,
    y: 0,
    duration: 1,
    ease: 'power2.out',
    scrollTrigger: {
      trigger: frame,
      start: 'top 80%',
      end: 'top 30%',
      scrub: 1
    }
  });

  // Parallax on full images
  const img = frame.querySelector('img');
  if (frame.classList.contains('full') && img) {
    const speed = parseFloat(frame.dataset.speed) || 1;
    gsap.fromTo(img,
      { yPercent: -10 * speed },
      {
        yPercent: 10 * speed,
        ease: 'none',
        scrollTrigger: {
          trigger: frame,
          start: 'top bottom',
          end: 'bottom top',
          scrub: true
        }
      }
    );
  }
});

// Phrase fade-in
gsap.utils.toArray('.tribe-phrase p').forEach(p => {
  gsap.fromTo(p,
    { opacity: 0, y: 30 },
    {
      opacity: 1, y: 0,
      duration: 1,
      scrollTrigger: {
        trigger: p,
        start: 'top 75%',
        end: 'top 40%',
        scrub: 1
      }
    }
  );
});
```

**Step 3: Add Dance and Invitation animations**

```javascript
/* === SCENE 4: THE DANCE === */
gsap.to('.dance-content', {
  opacity: 1,
  duration: 1,
  scrollTrigger: {
    trigger: '#dance',
    start: 'top 60%',
    end: 'top 20%',
    scrub: 1
  }
});

// Subtle parallax on dance background
gsap.fromTo('.dance-bg img',
  { scale: 1.1 },
  {
    scale: 1,
    scrollTrigger: {
      trigger: '#dance',
      start: 'top bottom',
      end: 'bottom top',
      scrub: true
    }
  }
);

/* === SCENE 5: THE INVITATION === */
gsap.to('.invitation-content', {
  opacity: 1,
  y: 0,
  duration: 1,
  scrollTrigger: {
    trigger: '#invitation',
    start: 'top 70%',
    toggleActions: 'play none none reverse'
  }
});
gsap.set('.invitation-content', { y: 40 });
```

**Step 4: Add Inner Circle logic (preserved from current site)**

```javascript
/* === INNER CIRCLE GATE === */
const icTrigger = document.getElementById('ic-trigger');
const icOverlay = document.getElementById('ic-overlay');
const icCloseX = document.getElementById('ic-close-x');
const icPassword = document.getElementById('ic-password');
const icSubmit = document.getElementById('ic-submit');
const icError = document.getElementById('ic-error');
const icGate = document.getElementById('ic-gate');
const icContent = document.getElementById('ic-content');
const icCloseBtn = document.getElementById('ic-close-btn');

function openIC() { icOverlay.classList.add('active'); }
function closeIC() { icOverlay.classList.remove('active'); }

icTrigger.addEventListener('click', openIC);
icCloseX.addEventListener('click', closeIC);
if (icCloseBtn) icCloseBtn.addEventListener('click', closeIC);
icOverlay.addEventListener('click', e => { if (e.target === icOverlay) closeIC(); });

icSubmit.addEventListener('click', () => {
  if (icPassword.value === 'blackopal2026') {
    icGate.style.display = 'none';
    icContent.classList.add('visible');
  } else {
    icError.textContent = 'Invalid access code';
    icPassword.style.borderColor = 'var(--magenta)';
    setTimeout(() => {
      icError.textContent = '';
      icPassword.style.borderColor = 'rgba(255,255,255,0.15)';
    }, 2000);
  }
});

icPassword.addEventListener('keydown', e => {
  if (e.key === 'Enter') icSubmit.click();
});
```

**Step 5: Verify the complete page works**

Open in browser. Test:
- Portal animation plays on load
- Scroll reveals awakening text, then robot image
- Tribe images parallax in
- Phrases appear with gradient text
- Dance section reveals
- Invitation form is functional
- Inner circle password gate works

**Step 6: Commit**

```bash
git add index.html
git commit -m "feat: add scroll animations for all scenes and inner circle gate"
```

---

### Task 6: Polish and Performance

Final adjustments: image loading states, performance tuning, mobile testing.

**Files:**
- Modify: `index.html`

**Step 1: Add image loading optimization**

Add to CSS:
```css
/* Smooth image reveal on load */
.tribe-frame img,
.awakening-image img,
.dance-bg img {
  transition: opacity 0.5s ease;
}
```

**Step 2: Add `<picture>` fallbacks if WebP was generated**

If WebP files exist in assets/, update image tags to:
```html
<picture>
  <source srcset="assets/robots/tribes-desert-duo.webp" type="image/webp">
  <img src="assets/robots/tribes-desert-duo.png" alt="..." loading="lazy">
</picture>
```

If no WebP files exist, skip this step - PNG works fine.

**Step 3: Test on mobile viewport (Chrome DevTools)**

Verify:
- Split grids collapse to single column on mobile
- Typography scales down properly
- Touch scrolling is smooth
- No horizontal overflow
- Images fill viewport correctly

**Step 4: Test inner circle gate**

- Click "Enter The Inner Circle"
- Enter wrong password -> error shown
- Enter "blackopal2026" -> content revealed
- Close button works

**Step 5: Final commit**

```bash
git add -A
git commit -m "polish: image loading, responsive fixes, performance tuning"
```

---

### Task 7: Retry Miro Downloads (when unblocked)

After the IP block lifts (~15-60 min), re-run the download script to get remaining images.

**Files:**
- Run: `materiales/download_all_miro.js`

**Step 1: Test if Miro is unblocked**

Try the MCP tool `mcp__miro__image_get_url` with any image. If it returns a URL, proceed.

**Step 2: Update the download script for safer rate limiting**

Edit `materiales/download_all_miro.js`:
- Set `CONCURRENCY = 2`
- Set delay between batches to `3000` ms
- The script already has resume logic (reads existing urls.json)

**Step 3: Run the download**

```bash
node materiales/download_all_miro.js
```

**Step 4: After download, curate additional robot images**

Review new images and add the best ones to `assets/robots/`. Update HTML img tags as needed.

**Step 5: Commit new assets**

```bash
git add assets/
git commit -m "assets: add additional robot images from Miro board"
```
