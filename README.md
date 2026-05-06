# ◈ ASCII HANDS — Gestural HUD Interface

> A fullscreen, zero-dependency web app that turns your webcam into a sci-fi gesture controller — rendering everything in ASCII art on a pure black canvas.

![ASCII HANDS](https://img.shields.io/badge/ASCII-HANDS-white?style=flat-square&labelColor=000&color=fff)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Hands-white?style=flat-square&labelColor=000&color=fff)
![Vanilla JS](https://img.shields.io/badge/Vanilla-JS-white?style=flat-square&labelColor=000&color=fff)
![No Build](https://img.shields.io/badge/No_Build-Required-white?style=flat-square&labelColor=000&color=fff)

---

## ✦ What It Does

Point your index fingers at a webcam and watch an ASCII 3D spinning globe materialize between them. Attach it to your hand, throw it across the room, and watch it explode into a particle shower — followed by a glowing ASCII butterfly.

All rendered in `Courier New` on a black canvas, with CRT scanlines and a futuristic HUD overlay.

---

## ✦ Features

| Feature | Description |
|---|---|
| **Hand Tracking** | Real-time 2-hand tracking via MediaPipe Hands (CDN, no install) |
| **4 Gestures** | `POINT` `PALM` `FIST` `OTHER` — classified per hand per frame |
| **5 States** | `IDLE → SUMMON → STICK → BLAST → BUTTERFLY` |
| **ASCII Globe** | Z-buffered ray-cast sphere with perspective projection + lighting |
| **Particle System** | 100-particle ASCII explosion with gravity, friction, decay |
| **ASCII Butterfly** | 2-frame wing-flap animation with fade-in glow |
| **Futuristic HUD** | Status, FPS counter, gesture chips, state messages, corner brackets |
| **CRT Aesthetic** | Scanline CSS overlay, glow/shadowBlur on all elements |
| **Embed Panel** | One-click `<iframe>` code copy for embedding |

---

## ✦ Gesture Guide

```
POINT  = only index finger extended
PALM   = all four fingers extended
FIST   = no fingers extended
OTHER  = anything else
```

---

## ✦ State Machine

```
         BOTH INDEX FINGERS
  IDLE ─────────────────────► SUMMON
   ▲                          │    │
   │           RIGHT PALM     │    │ BOTH FISTS
   │          ┌───────────────┘    │
   │          ▼                    ▼
   │         STICK ──────────► BLAST
   │          │    BOTH FISTS    │
   │          │                  ▼
   │          │             BUTTERFLY
   │          │                  │
   └──────────┴──────────────────┘
               after 210 frames / otherwise
```

---

## ✦ Quick Start

**No install, no build step.**

### Option A — Open locally
```bash
git clone https://github.com/YOUR_USERNAME/ascii-hands.git
cd ascii-hands
# Serve over HTTPS (required for camera access)
npx serve .
# or
python3 -m http.server 8080
# then open https://localhost:8080
```

> ⚠️ Camera access requires either `localhost` or an HTTPS origin. Plain `file://` URLs will not work.

### Option B — Deploy to GitHub Pages
1. Go to your repo **Settings → Pages**
2. Set source: `main` branch, `/ (root)`
3. GitHub Pages automatically serves over HTTPS ✓

### Option C — Deploy to Netlify / Vercel
Drop the folder — it works out of the box with no configuration.

---

## ✦ Tech Stack

- **[MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)** — real-time hand landmark detection (loaded via jsDelivr CDN)
- **HTML5 Canvas 2D** — all rendering (globe, particles, cursors, HUD)
- **Vanilla JavaScript** — zero frameworks, zero build tools
- **`requestAnimationFrame`** render loop + `setInterval` at ~30fps for ML inference

---

## ✦ How the Globe Works

The globe uses a classic **z-buffer ASCII ray-casting** approach:

1. Iterate over spherical coordinates `(θ, φ)`
2. Compute surface normal `(x, y, z)` on unit sphere
3. Apply two rotation matrices (Y-axis spin + X-axis tilt)
4. Project with perspective: `ooz = 1 / (z/K + 1.4)`
5. Map to character grid, keeping highest `ooz` per cell (z-buffer)
6. Map luminance (dot product with light vector) to character set:  
   `' .,:;!|+oxO0#@'`

---

## ✦ File Structure

```
ascii-hands/
└── index.html    ← entire app, single file, ~450 lines
└── README.md
```

---

## ✦ Browser Support

| Browser | Status |
|---|---|
| Chrome 90+ | ✅ Full support |
| Edge 90+ | ✅ Full support |
| Firefox | ⚠️ May have MediaPipe issues |
| Safari | ⚠️ Camera API restrictions on some versions |
| Mobile Chrome | ✅ Works (use front camera) |

---

## ✦ Customization

All key parameters are at the top of the `<script>` block:

```js
const FINGER_THRESH    = 1.45;  // gesture sensitivity
const LERP_SUMMON      = 0.12;  // globe follow speed (SUMMON state)
const LERP_STICK       = 0.20;  // globe follow speed (STICK state)
const BLAST_FRAMES     = 38;    // frames before BUTTERFLY transition
const BUTTERFLY_FRAMES = 210;   // frames before returning to IDLE
```

---

## ✦ License

MIT — do whatever you want with it.

---

<p align="center">
  Built with ◈ and <code>Courier New</code>
</p>
