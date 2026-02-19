# Glimmer

**A Claude Code skill for designing transparent, glassmorphic, and AR-inspired user interfaces.**

[**Live Demo & Documentation**](https://matrixy.github.io/Glimmer/)

---

Glimmer teaches Claude how to create interfaces that feel like light projected onto reality — not rectangles on a screen. Grounded in real physics and human factors of transparent display design, it produces UIs that look futuristic *and* actually work.

> "There's no screen, no crisp blank page. Instead, you're faced with the world itself."

## Design Principles

| # | Principle | Summary |
|---|-----------|---------|
| 1 | **Dark surfaces, bright content** | Never light backgrounds with dark text — causes halation on additive displays |
| 2 | **Additive color model** | Desaturated, high-luminance accents only; no saturated primaries |
| 3 | **Glanceable typography** | Medium-to-bold weights (500+), wide letter-spacing, 16px minimum body size |
| 4 | **Depth via dark shadows** | Rich multi-layer dark shadows, luminous edge borders — not Material elevation |
| 5 | **Ambient motion** | Slow materialization entrances (1.2s), instant interaction feedback (<150ms) |
| 6 | **Bright focus states** | Glowing cyan outlines for focus, luminance increase on hover |

## Supported Platforms

| Platform | Blur Method | Notes |
|----------|------------|-------|
| **Web / React** | `backdrop-filter: blur()` | CSS custom properties for design tokens |
| **SwiftUI** | `.ultraThinMaterial` | visionOS uses `.glassBackgroundEffect()` |
| **Jetpack Compose** | Solid dark surfaces (pre-API 31) | Android XR uses Compose Glimmer natively |
| **React Native** | `@react-native-community/blur` / `expo-blur` | Android falls back to solid dark surfaces |

## Installation

Install with a single command via [skills.sh](https://skills.sh):

```bash
npx skills add MaTriXy/Glimmer
```

Then ask Claude for a "glass UI", "transparent interface", "HUD dashboard", "holographic overlay", or any of the trigger keywords — the skill activates automatically.

## Trigger Keywords

`glass UI` · `transparent UI` · `HUD` · `holographic` · `glassmorphism` · `frosted glass` · `visionOS` · `futuristic interface` · `AR mockup` · `heads-up display` · `smart glasses UI` · `sci-fi UI` · `floating UI` · `ambient display`

## Repo Structure

```
SKILL.md                          # Main skill definition — design system + principles
references/
  components.md                   # CSS component catalog (Card, Button, Input, etc.)
  native-platforms.md             # SwiftUI, Jetpack Compose, React Native implementations
docs/                             # GitHub Pages site with live demo
```

## Live Demo

The [GitHub Pages site](https://matrixy.github.io/Glimmer/) showcases:

- Interactive glass component demo with 6 scene backgrounds
- Live-rendered buttons, inputs, status indicators, gauges, and data bars
- Full documentation of the design system
- Platform-specific code examples

## Inspiration

- The quiet confidence of a pilot's HUD
- The ambient elegance of a luxury car's head-up display
- The sci-fi sophistication of *Blade Runner 2049*, *Her*, and *Iron Man*
- Google's Jetpack Compose Glimmer design system for Android XR

---

**Glimmer** — interfaces that feel like light woven into reality.
