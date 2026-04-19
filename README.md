<p align="center">
  <img src="https://img.shields.io/badge/Claude%20Code-Skill-10b981?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code Skill">
  <img src="https://img.shields.io/badge/license-MIT-blue?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/single--file-HTML-orange?style=flat-square" alt="Single File">
</p>

<h1 align="center">Claude Algorithm Visualizer</h1>

<p align="center">
  <b>Turn any LeetCode / algorithm solution into a cinematic, interactive step-by-step animation with one command.</b>
</p>

<p align="center">
  <a href="README.zh.md"><img src="https://img.shields.io/badge/中文文档-Read-blue?style=flat-square" alt="中文文档"></a>
</p>

---

## Demo

<p align="center">
  <img src="./assets/demo.gif" width="90%" alt="Sort Colors Animation Demo">
</p>

---

## Features

| Feature | Description |
|---------|-------------|
| **One-command generation** | Paste your code + test case → get a `.html` demo instantly |
| **Cinematic dark UI** | Deep-space dark theme with neon accents, inspired by professional IDEs |
| **Synced code highlighting** | Active line follows the animation frame-by-frame |
| **Smart state cards** | Human-readable Chinese explanations of every step |
| **Zero dependencies** | Single self-contained `.html` file — no build step, no CDN |
| **Full playback controls** | Play / Pause / Prev / Next / Reset / Speed (0.2x–2.0x) |
| **Keyboard-driven** | Arrow keys + Spacebar for seamless scrubbing |
| **Responsive layout** | Collapses gracefully on screens < 900px |
| **Multi-pattern support** | Single array, dual arrays, 2D grids, linked lists, trees |

---

## Quick Start

### 1. Install the Skill

Place the `skill/` folder (or the packaged `.skill` file) into your Claude Code skills directory, then restart Claude Code.

```bash
# macOS / Linux
~/.claude/skills/algo-visualizer/

# Windows
%USERPROFILE%\.claude\skills\algo-visualizer\
```

### 2. Invoke

In any Claude Code conversation, simply type:

```
/algo-visualizer
```

Then paste:
- Your **LeetCode problem number / title** (optional, for context)
- Your **solution code**
- A **small test case** (5–10 elements)

Claude will generate a `problem-name-demo.html` file ready to open in any browser.

### 3. Record & Share

Open the HTML, hit **Play**, and use any screen recorder (OBS, LICEcap, ScreenToGif) to capture the animation. Perfect for:
- <img src="https://img.shields.io/badge/YouTube-Tutorial-ff0000?style=flat-square&logo=youtube"> Algorithm explanation videos
- <img src="https://img.shields.io/badge/X/Twitter-Thread-000000?style=flat-square&logo=x"> Viral tech threads
- <img src="https://img.shields.io/badge/Resume-Portfolio-0077b5?style=flat-square&logo=linkedin"> Portfolio pieces

---

## Supported Models

| Platform | Status | Notes |
|----------|--------|-------|
| **Claude Code (CLI)** | ✅ Fully supported | Primary target; invoke via `/algo-visualizer` |
| **Claude 4.x (Sonnet/Opus)** | ✅ Fully supported | Best-in-class code generation & reasoning |
| **Claude 3.5/3.7 Sonnet** | ✅ Supported | Slightly more steering may be needed for complex DP |
| **Other Claude interfaces** | ⚠️ Manual load | Copy `SKILL.md` into system prompt |

---

## Optimization Highlights

> Designed for **context-window efficiency** and **visual impact**.

1. **Progressive Disclosure Architecture**
   - SKILL.md only loads core workflow (~8KB); heavy boilerplate lives in `assets/template.html`
   - Prevents context-bloat when multiple skills are active

2. **Token-Efficient CSS Framework**
   - CSS custom properties (`:root` variables) ensure consistent theming without repetition
   - All animations are GPU-composited (`transform`, `opacity`) for 60fps smoothness

3. **Deterministic Step Engine**
   - `buildSteps()` simulates the algorithm in JS rather than using `eval()` — safe, predictable, and debuggable
   - Each step is a pure state snapshot; `render()` is idempotent

4. **Motion Design System**
   - Cubic-bezier `[0.34, 1.56, 0.64, 1]` for bouncy pointer transitions
   - Staggered `sortedPop` animations on completion for a "reward" feeling
   - Pulsing yellow `borderPulse` on comparison hotspots

5. **Accessibility & UX**
   - Keyboard shortcuts reduce friction for presenters
   - Speed slider (0.2x–2.0x) adapts to audience expertise
   - Status messages in Chinese match the primary user demographic

---

## Project Structure

```
claude-algo-visualizer/
├── skill/                          # Claude Code Skill package
│   ├── SKILL.md                    # Skill manifest & workflow instructions
│   ├── _meta.json                  # Skill metadata
│   └── assets/
│       └── template.html           # Reusable HTML boilerplate (CSS + JS engine)
├── demo/
│   └── sort-colors-demo.html     # LeetCode 75 — Sort Colors (Dutch National Flag)
├── README.md                       # This file (English)
├── README.zh.md                    # Chinese documentation
└── LICENSE                         # MIT
```

---

## Demo Gallery

| Problem | Algorithm | File |
|---------|-----------|------|
| LeetCode 75 — Sort Colors | Three-way partition | `demo/sort-colors-demo.html` |
| LeetCode 1855 — Max Distance | Monotonic two pointers | *(generate with skill)* |
| LeetCode 3 — Longest Substring | Sliding window | *(generate with skill)* |

> Want your demo featured? Open a PR with your generated `.html`!

---

## License

MIT © 2026

---

<p align="center">
  If this skill helps you land an offer or go viral, give it a ⭐ and share the story!
</p>
