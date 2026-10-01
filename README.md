# Cyber-Avian Studio — Master Pose & Color Calibration

An interactive 3D studio and frame-sequence export workstation built with [Three.js](https://threejs.org/), [lil-gui](https://lil-gui.georgealways.com/), and [JSZip](https://stuk.github.io/jszip/). It renders high-poly `.glb` models into deterministic, high-efficiency WebP image sequences for buttery-smooth 60+ FPS scroll animations powered by GSAP and HTML5 Canvas.

---

## 🚀 Key Features

- **Interactive 3D Orbit & Pose Calibration:** Toggle between **Timeline Scrub** mode (preview transitions 0–200) and **3D Orbit Edit** mode (drag and rotate directly on canvas to define new keyframes).
- **5 Section Checkpoints:** Calibrated keyframe poses for storytelling sections:
  - `Hero` (Frame 0): Dynamic 3/4 banking dive toward lower-left.
  - `About` (Frame 50): Symmetrical overhead dive from upper center.
  - `Projects` (Frame 100): Dead-level horizontal runway wingspan.
  - `Experience` (Frame 150): Left-docked 90-degree profile facing right.
  - `Studies` (Frame 200): Deep overhead dive pinned to the top horizon.
- **Radial Vignette Background:** Dynamic procedural canvas gradient from center slate (`#30333a`) to charcoal edge (`#0c0d10`).
- **Studio Lighting Rig:** Dual key lights (left & right), top rim light (`#ffb555`), and ambient fill with a global master multiplier.
- **PBR Material Tuning:** Configurable orange neon emissive intensity (`3.8`), sRGB encoding, and tone mapping with `ACESFilmicToneMapping`.
- **In-Browser Sequence Exporter:** Batches all 201 frames into WebP images (`image/webp`, 0.90 quality) and downloads `cyber_avian_201_frames.zip` via JSZip.

---

## 📂 Project Structure

```
├── eagle.glb               # Primary 3D binary model
├── exporter.html           # Main studio application & WebGL exporter
├── EXPORTER_GUIDE.md       # In-depth architectural & curriculum breakdown
├── docs/
│   └── GUIDE.md            # Onboarding & configuration reference
└── README.md               # Quickstart and overview
```

---

## 🛠️ Quickstart

Due to browser CORS policies on WebGL assets (`.glb`), open the project via a local HTTP server:

```bash
# Using Node (npx)
npx serve .

# OR using Python 3
python3 -m http.server 8000
```

Navigate to `http://localhost:3000/exporter.html` (or `http://localhost:8000/exporter.html`).

---

## 🎮 Controls & GUI Guide

| Control | Mode | Action |
| :--- | :--- | :--- |
| **Mouse Wheel** | `Timeline Scrub` | Scrubs timeline between frame `0` and `200`. |
| **Timeline Scrubber** | Any | Drag range input to jump to any frame. |
| **Canvas Drag** | `3D Orbit Edit` | Rotate / pan around model in real-time. |
| **Jump Section** | GUI | Snaps directly to `hero`, `about`, `projects`, `experience`, or `studies`. |
| **💾 Save Orbit to Section** | GUI | Captures current Orbit camera coordinates into the active section. |
| **📋 Copy JSON Poses** | GUI | Copies the complete `sectionPoses` JSON to clipboard for easy saving. |
| **EXPORT 201 WEBP FRAMES** | Button | Automatically renders all 201 frames and downloads `.zip`. |

---

## ⚡ Integration with GSAP + HTML5 Canvas

Unpack the exported `sequence/` folder (`frame_000.webp` to `frame_200.webp`) into your web application's `/public` folder:

```html
<div id="scroll-track" style="height: 500vh; position: relative;">
  <canvas id="hero-canvas" style="position: sticky; top: 0; width: 100vw; height: 100vh; object-fit: contain;"></canvas>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>
<script>
  gsap.registerPlugin(ScrollTrigger);

  const canvas = document.getElementById("hero-canvas");
  const ctx = canvas.getContext("2d");
  canvas.width = 1280;
  canvas.height = 720;

  const totalFrames = 201;
  const currentFrame = (i) => `/sequence/frame_${String(i).padStart(3, "0")}.webp`;

  const images = [];
  const playhead = { frame: 0 };

  for (let i = 0; i < totalFrames; i++) {
    const img = new Image();
    img.src = currentFrame(i);
    images.push(img);
  }

  images[0].onload = () => ctx.drawImage(images[0], 0, 0);

  gsap.to(playhead, {
    frame: totalFrames - 1,
    snap: "frame",
    ease: "none",
    scrollTrigger: {
      trigger: "#scroll-track",
      start: "top top",
      end: "bottom bottom",
      scrub: 0.5
    },
    onUpdate: () => {
      const img = images[Math.round(playhead.frame)];
      if (img && img.complete) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.drawImage(img, 0, 0);
      }
    }
  });
</script>
```

---

## 📖 In-Depth Documentation

For full mathematical formulas, PBR lighting theory, and chapter choreography tables, consult:
- [EXPORTER_GUIDE.md](file:///home/drone/eagle-render/EXPORTER_GUIDE.md)
- [docs/GUIDE.md](file:///home/drone/eagle-render/docs/GUIDE.md)
