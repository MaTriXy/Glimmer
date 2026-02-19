---
name: glimmer
description: >
  Design and build stunning transparent, glassmorphic, and AR-inspired user interfaces
  that simulate the look and feel of heads-up displays, smart glasses, and holographic
  overlays — across web (HTML/CSS/React), SwiftUI, Jetpack Compose, and React Native.
  Use this skill whenever the user asks for glass UI, transparent UI, HUD interfaces,
  AR/XR mockups, holographic dashboards, futuristic interfaces, sci-fi UI, heads-up
  displays, smart glasses UI, visionOS interfaces, or any interface that should feel
  like it's floating over the real world. Also trigger when users mention "glassmorphism",
  "frosted glass", "translucent UI", "ambient display", "overlay interface",
  ".ultraThinMaterial", "Material3 transparent", or want to create interfaces inspired
  by Google Glass, Apple Vision Pro, Meta glasses, Android XR, or any wearable display
  technology. Trigger for native mobile glass effects in SwiftUI, Jetpack Compose, or
  React Native. Even if the user just says "make it look futuristic" or "floating UI"
  — this is the skill to use.
---

# Transparent Glass UI

Create interfaces that feel like light projected onto reality — not rectangles on a screen.

This skill is grounded in the real physics and human factors of transparent display design, distilled from years of work on additive displays (like AR glasses). These principles produce UIs that look breathtakingly futuristic *and* actually work — because they're rooted in how light, color, and human vision truly behave.

## Core Philosophy

> "There's no screen, no crisp blank page. Instead, you're faced with the world itself."

Traditional UI design starts with a container — a screen, a rectangle to fill. Transparent glass UI starts with **the world as the canvas**. Every design decision flows from this inversion:

- **Light is your medium, not pixels.** You're adding luminance, not painting surfaces.
- **Black is transparency.** On additive displays, black = see-through. In our web simulations, dark surfaces create the illusion of looking *through* the interface.
- **The interface earns attention.** It appears when needed, disappears when not. Ambient, not aggressive.
- **Depth is real.** Elements exist at perceived distances, not just z-index layers.

## Design Principles

### 1. Dark Surfaces, Bright Content

This is the foundational rule. On real transparent displays, bright surfaces cause **halation** — light bleeds into adjacent content, making text illegible. The solution:

- Surfaces are always dark (near-black with slight transparency)
- Content (text, icons, accents) is always bright
- This creates maximum contrast against *any* background
- Never use light backgrounds with dark text — it's physically wrong for this medium

```css
/* ✅ CORRECT — dark surface, bright content */
.glass-card {
  background: rgba(0, 0, 0, 0.65);
  color: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(24px) saturate(1.2);
}

/* ❌ WRONG — light surface, dark content (causes halation) */
.glass-card-bad {
  background: rgba(255, 255, 255, 0.3);
  color: #333;
}
```

### 2. Additive Color Model

Colors must be close to white to maintain contrast on transparent surfaces. Saturated colors disappear against the real world. The palette should be desaturated and luminous.

- Use desaturated, high-luminance accent colors (pastels pushed toward white)
- Never use fully saturated primaries — they vanish against colorful backgrounds
- Neutral interface by default; color is used sparingly and purposefully
- Think of color as *adding light*, not *painting pigment*

```css
:root {
  /* Desaturated, luminous accent palette */
  --accent-cyan: #a0e6f5;
  --accent-coral: #ffbeb4;
  --accent-gold: #ffe1a0;
  --accent-lilac: #d2beff;
  --accent-mint: #b8f0d8;
  --accent-rose: #f5b8d0;
  
  /* Surface system — always dark */
  --surface-primary: rgba(0, 0, 0, 0.55);
  --surface-elevated: rgba(15, 15, 20, 0.75);
  --surface-overlay: rgba(0, 0, 0, 0.85);
  
  /* Content — always bright */
  --text-primary: rgba(255, 255, 255, 0.95);
  --text-secondary: rgba(255, 255, 255, 0.6);
  --text-tertiary: rgba(255, 255, 255, 0.35);
}
```

### 3. Typography for Glanceability

On transparent displays, text is measured in visual angle (degrees), not pixels. The web equivalent: text must be **bold, open, and spaced** to be glanceable against complex backgrounds.

- Use medium-to-bold weights only (400 minimum, 500-700 preferred)
- Increase letter-spacing significantly (+0.02em to +0.06em)
- Use fonts with large counters (open a, e, g) — Google Sans, Inter, or similar geometric sans-serifs with open apertures
- Minimum body text: 16px. Preferred: 18-20px.
- Never use thin/light weights — they become invisible
- Favor uppercase for labels and small UI elements (improves scanability)

```css
.glass-text-title {
  font-weight: 600;
  font-size: 1.75rem;
  letter-spacing: 0.04em;
  line-height: 1.2;
}

.glass-text-body {
  font-weight: 500;
  font-size: 1.125rem;
  letter-spacing: 0.02em;
  line-height: 1.5;
}

.glass-text-label {
  font-weight: 600;
  font-size: 0.75rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}
```

### 4. Depth Through Shadow, Not Elevation

Traditional Material shadows don't work on transparent displays — light surfaces can't cast dark shadows convincingly. Instead, use **dark, rich shadows** that create a sense of occlusion:

- Shadows should be dark and spread wide (not the typical light gray)
- Use multiple shadow layers for depth
- Inner glows and edge highlights replace traditional borders
- Subtle luminous borders (1px bright lines) convey edges

```css
.glass-card {
  /* Rich, dark shadow for depth */
  box-shadow: 
    0 8px 32px rgba(0, 0, 0, 0.5),
    0 2px 8px rgba(0, 0, 0, 0.3),
    inset 0 0.5px 0 rgba(255, 255, 255, 0.1);
  
  /* Luminous edge — the "glass edge catch" */
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-top: 1px solid rgba(255, 255, 255, 0.15);
}
```

### 5. Motion: Graceful, Not Grabby

On heads-up displays, motion must guide attention without startling. The principles:

- **Ambient transitions are slow** (1.5–2 seconds) — incoming notifications, status changes
- **Response to input is instant** (<150ms) — button presses, selections
- **Fade-in from transparency** is the primary entrance pattern (not slide-in)
- Elements should feel like they **materialize from light**, not fly in from off-screen
- Use ease-out curves for entrances, ease-in for exits
- Avoid bouncy/spring animations — they feel jarring in an ambient context

```css
/* Ambient entrance — slow, respectful */
@keyframes glass-materialize {
  from {
    opacity: 0;
    transform: scale(0.97) translateY(6px);
    filter: blur(6px);
  }
  to {
    opacity: 1;
    transform: scale(1) translateY(0);
    filter: blur(0);
  }
}

.glass-card {
  animation: glass-materialize 1.2s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

/* Instant feedback for interaction */
.glass-button:active {
  transition: all 0.08s ease-out;
  background: rgba(255, 255, 255, 0.15);
}
```

### 6. Focus Rings and Interaction Feedback

Because the interface competes with the real world for attention, interaction states must be unmistakable:

- **Focus rings**: Bright, glowing outlines (not browser defaults)
- **Hover states**: Subtle luminance increase on the surface
- **Active states**: Immediate brightness flash
- **Selected states**: Persistent bright accent border or glow

```css
.glass-interactive:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(255, 255, 255, 0.15);
}

.glass-interactive:focus-visible {
  outline: none;
  box-shadow: 
    0 0 0 2px rgba(160, 230, 245, 0.6),
    0 0 20px rgba(160, 230, 245, 0.15);
}
```

## Implementation Guide

### Background Simulation

Since we're on the web (not real AR glasses), simulate the transparent display effect:

1. **Use a rich photographic or gradient background** behind the UI to simulate the "real world" showing through
2. **Apply `backdrop-filter: blur()`** on glass surfaces to create the frosted-glass effect
3. **Layer a subtle noise texture** over glass surfaces for physical realism
4. **Consider using a looping video or animated gradient** as the background for maximum impact

```css
body {
  background: url('world-background.jpg') center/cover no-repeat fixed;
  min-height: 100vh;
}

/* Optional: animated gradient as background */
body::before {
  content: '';
  position: fixed;
  inset: 0;
  background: linear-gradient(
    135deg,
    #0a0a1a 0%,
    #1a0a2e 25%,
    #0a1628 50%,
    #0d1f0d 75%,
    #1a0a1a 100%
  );
  background-size: 400% 400%;
  animation: gradientShift 20s ease infinite;
  z-index: -1;
}
```

### Glass Card Component (Reference Implementation)

```html
<div class="glass-card">
  <div class="glass-card-header">
    <span class="glass-label">NAVIGATION</span>
    <span class="glass-status">● ACTIVE</span>
  </div>
  <h2 class="glass-title">N Shoreline Blvd</h2>
  <p class="glass-body">450m ahead — Turn right</p>
  <div class="glass-card-actions">
    <button class="glass-button glass-button-primary">Directions</button>
    <button class="glass-button glass-button-ghost">Dismiss</button>
  </div>
</div>
```

### Responsive Considerations

Glass UI should feel **ambient and contained**, never sprawling:

- Max card width: 480px (simulates the limited field-of-view of glasses)
- Content should breathe — generous padding (24-32px)
- On mobile, cards should feel like they float over the viewport
- Use `position: fixed` or sticky positioning to anchor key UI elements

### Accessibility

Transparent UIs have unique accessibility challenges:

- Maintain WCAG AA contrast ratios (4.5:1 for normal text) — the dark-surface-bright-content model naturally achieves this
- Ensure backdrop-filter fallback (solid dark background) for browsers that don't support it
- Focus states must be highly visible (the glowing focus ring pattern)
- Respect `prefers-reduced-motion` — replace animations with simple fades
- Provide `prefers-contrast: more` overrides that increase surface opacity

```css
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; }
  .glass-card { animation: none; opacity: 1; }
}

@media (prefers-contrast: more) {
  :root {
    --surface-primary: rgba(0, 0, 0, 0.9);
    --text-primary: rgba(255, 255, 255, 1);
  }
}
```

## Component Library

For detailed component specifications and ready-to-use code, read:

```
Read references/components.md    # Full component catalog with code
```

Components include: Glass Card, Glass Button, Glass Input, Glass Notification (with ambient entrance), Glass Navigation Bar, Glass Status Indicator, Glass Modal/Overlay, Glass Data Visualization, and HUD Reticle elements.

## Platform-Specific Implementation

The core design principles (dark surfaces, bright content, additive color, ambient motion) apply universally. The implementation details differ per platform.

### Platform Selection

| Platform | When to use | Reference |
|----------|-------------|-----------|
| **HTML/CSS/React** | Web apps, artifacts, prototypes | `references/components.md` |
| **SwiftUI** | iOS, macOS, visionOS apps | `references/native-platforms.md` → SwiftUI section |
| **Jetpack Compose** | Android, Android XR apps | `references/native-platforms.md` → Compose section |
| **React Native** | Cross-platform mobile apps | `references/native-platforms.md` → React Native section |

When the user specifies a platform, read the appropriate reference file for platform-native code and API usage. When no platform is specified, default to React/HTML for artifacts.

```
Read references/native-platforms.md    # SwiftUI, Compose, React Native implementations
```

## Anti-Patterns (What NOT to Do)

- ❌ White or light-colored card backgrounds (causes halation)
- ❌ Thin/light font weights (invisible on busy backgrounds)
- ❌ Fully saturated colors (vanish against the real world)
- ❌ Traditional Material-style raised shadows
- ❌ Slide-in animations from screen edges (feels like phone UI, not AR)
- ❌ Dense information layouts (the interface should be glanceable, not a spreadsheet)
- ❌ Browser-default focus rings (invisible on glass surfaces)
- ❌ Rounded rectangles with visible borders on bright backgrounds (looks like a sticker, not glass)

## Inspiration & Mood

When generating transparent glass UI, channel:
- The quiet confidence of a pilot's HUD
- The ambient elegance of a luxury car's head-up display
- The sci-fi sophistication of interfaces in *Blade Runner 2049*, *Her*, or *Iron Man*
- The calm utility of a surgeon's AR overlay
- Google's Jetpack Compose Glimmer design system for Android XR

The goal is an interface that feels like **light woven into reality** — not a screen imposed upon it.
