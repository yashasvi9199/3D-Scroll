# Eagle Render — Developer & Project Guide

## 1. Project Overview
`eagle-render` is an asset generation workstation designed to pre-render 3D GLTF/GLB models (`eagle.glb`) into optimized frame sequences (WebP) for ultra-high-performance scroll-driven web animations (Canvas + GSAP ScrollTrigger).

- Primary Workstation File: [exporter.html](file:///home/drone/eagle-render/exporter.html)
- 3D Model: [eagle.glb](file:///home/drone/eagle-render/eagle.glb)
- In-depth Curriculum & Parameter Breakdown: [EXPORTER_GUIDE.md](file:///home/drone/eagle-render/EXPORTER_GUIDE.md)

---

## 2. Onboarding & Local Setup

### Prerequisites
- Node.js (with `pnpm` or `npx`) OR Python 3.

### Running the Exporter
Because modern web browsers enforce CORS security policies on local files, `exporter.html` cannot be opened via `file://`. Serve it over HTTP:

```bash
# Option A: Instant Node serve
npx serve .

# Option B: Python HTTP server
python3 -m http.server 8000
```
Access the application in your browser at `http://localhost:3000` or `http://localhost:8000/exporter.html`.

---

## 3. Workflow & Usage

1. **Load Model:** `exporter.html` loads `./eagle.glb` automatically and normalizes its dimensions.
2. **Review Geometry:** Use the `FLIP 180°` button if the model faces backward.
3. **Scrub Sequence:** Use the timeline slider to preview the 5 narrative chapters (Hero, About, Projects, Experience, Credentials).
4. **Export Sequence:** Click `EXPORT WEBP FRAMES (.ZIP)` to render all frames and download `cyber_avian_sequence.zip`.
5. **Web Integration:** Unpack the frames into your frontend project's `/public/sequence/` directory and bind to `<canvas>` via GSAP ScrollTrigger. Full copy-paste snippet available in [EXPORTER_GUIDE.md](file:///home/drone/eagle-render/EXPORTER_GUIDE.md#step-5-implement-in-your-web-app-gsap--canvas).

---

## 4. Key Configurations

| Config Parameter | File Reference | Default | Impact |
| :--- | :--- | :--- | :--- |
| `TOTAL_FRAMES` | [exporter.html:141](file:///home/drone/eagle-render/exporter.html#L141) | `90` | Number of rendered frames. |
| `CANVAS_WIDTH` / `CANVAS_HEIGHT` | [exporter.html:149-150](file:///home/drone/eagle-render/exporter.html#L149-L150) | `1280x720` | Output frame resolution. |
| `BG_COLOR` | [exporter.html:157](file:///home/drone/eagle-render/exporter.html#L157) | `0xcfd5dc` | Solid background color. |
| `toneMappingExposure` | [exporter.html:194](file:///home/drone/eagle-render/exporter.html#L194) | `1.5` | Exposure/brightness gain. |
| `metalness` / `roughness` | [exporter.html:310,319](file:///home/drone/eagle-render/exporter.html#L310) | `0.5` / `0.90` | PBR surface response. |
| `emissiveIntensity` | [exporter.html:328](file:///home/drone/eagle-render/exporter.html#L328) | `3.8` | Glow on LED sensors & thrusters. |

Refer to [EXPORTER_GUIDE.md](file:///home/drone/eagle-render/EXPORTER_GUIDE.md) for full parameter formulas, coordinate breakdowns, and enhancement strategies.
