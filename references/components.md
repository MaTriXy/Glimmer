# Transparent Glass UI — Component Reference

A complete catalog of glass UI components with production-ready code.

## Table of Contents
1. [CSS Foundation](#css-foundation)
2. [Glass Card](#glass-card)
3. [Glass Button](#glass-button)
4. [Glass Input](#glass-input)
5. [Glass Notification](#glass-notification)
6. [Glass Navigation Bar](#glass-navigation-bar)
7. [Glass Status Indicator](#glass-status-indicator)
8. [Glass Modal](#glass-modal)
9. [Glass Data Visualization](#glass-data-visualization)
10. [HUD Reticle Elements](#hud-reticle)
11. [Glass Chip / Tag](#glass-chip)
12. [Glass Progress / Loading](#glass-progress)

---

## CSS Foundation

Always include these base styles. They establish the additive-display simulation.

```css
/* === GLASS UI FOUNDATION === */

@import url('https://fonts.googleapis.com/css2?family=DM+Sans:opsz,wght@9..40,400;9..40,500;9..40,600;9..40,700&display=swap');

:root {
  /* Surface system — dark, transparent */
  --glass-surface-1: rgba(0, 0, 0, 0.55);
  --glass-surface-2: rgba(15, 15, 20, 0.75);
  --glass-surface-3: rgba(0, 0, 0, 0.85);
  
  /* Content — bright, luminous */
  --glass-text-1: rgba(255, 255, 255, 0.95);
  --glass-text-2: rgba(255, 255, 255, 0.6);
  --glass-text-3: rgba(255, 255, 255, 0.35);
  
  /* Accent palette — desaturated, high-luminance */
  --glass-accent-cyan: #a0e6f5;
  --glass-accent-coral: #ffbeb4;
  --glass-accent-gold: #ffe1a0;
  --glass-accent-lilac: #d2beff;
  --glass-accent-mint: #b8f0d8;
  --glass-accent-rose: #f5b8d0;
  
  /* Glow variants (lower opacity for shadows/glows) */
  --glass-glow-cyan: rgba(160, 230, 245, 0.3);
  --glass-glow-coral: rgba(255, 190, 180, 0.3);
  
  /* Borders */
  --glass-border: rgba(255, 255, 255, 0.08);
  --glass-border-bright: rgba(255, 255, 255, 0.15);
  
  /* Shadows */
  --glass-shadow: 0 8px 32px rgba(0, 0, 0, 0.45), 0 2px 8px rgba(0, 0, 0, 0.25);
  --glass-shadow-elevated: 0 16px 48px rgba(0, 0, 0, 0.6), 0 4px 12px rgba(0, 0, 0, 0.3);
  
  /* Blur */
  --glass-blur: blur(24px) saturate(1.2);
  --glass-blur-heavy: blur(40px) saturate(1.4);
  
  /* Spacing */
  --glass-pad-xs: 8px;
  --glass-pad-sm: 12px;
  --glass-pad-md: 20px;
  --glass-pad-lg: 32px;
  
  /* Radius */
  --glass-radius-sm: 8px;
  --glass-radius-md: 16px;
  --glass-radius-lg: 24px;
  
  /* Timing */
  --glass-ease-ambient: cubic-bezier(0.16, 1, 0.3, 1);
  --glass-ease-responsive: cubic-bezier(0.2, 0, 0, 1);
  --glass-duration-ambient: 1.2s;
  --glass-duration-fast: 0.15s;
}

*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'DM Sans', system-ui, sans-serif;
  color: var(--glass-text-1);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Noise texture overlay for physical realism */
.glass-noise::after {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  opacity: 0.03;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
  pointer-events: none;
  mix-blend-mode: overlay;
}

/* Materialize entrance animation */
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

/* Ambient pulse for status indicators */
@keyframes glass-pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

/* Reduced motion fallback */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

@media (prefers-contrast: more) {
  :root {
    --glass-surface-1: rgba(0, 0, 0, 0.9);
    --glass-surface-2: rgba(0, 0, 0, 0.95);
    --glass-text-1: #fff;
    --glass-text-2: rgba(255, 255, 255, 0.8);
    --glass-border: rgba(255, 255, 255, 0.25);
  }
}
```

---

## Glass Card

The fundamental container. Dark surface, frosted blur, luminous edges.

```css
.glass-card {
  position: relative;
  background: var(--glass-surface-1);
  backdrop-filter: var(--glass-blur);
  -webkit-backdrop-filter: var(--glass-blur);
  border: 1px solid var(--glass-border);
  border-top-color: var(--glass-border-bright);
  border-radius: var(--glass-radius-md);
  padding: var(--glass-pad-lg);
  box-shadow: var(--glass-shadow);
  max-width: 480px;
  animation: glass-materialize var(--glass-duration-ambient) var(--glass-ease-ambient) forwards;
  overflow: hidden;
}

/* Fallback for no backdrop-filter support */
@supports not (backdrop-filter: blur(1px)) {
  .glass-card {
    background: rgba(10, 10, 20, 0.92);
  }
}

.glass-card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: var(--glass-pad-md);
}

.glass-card-title {
  font-weight: 600;
  font-size: 1.5rem;
  letter-spacing: 0.03em;
  line-height: 1.2;
  color: var(--glass-text-1);
}

.glass-card-subtitle {
  font-weight: 500;
  font-size: 1rem;
  letter-spacing: 0.02em;
  color: var(--glass-text-2);
  margin-top: 4px;
}

.glass-card-body {
  font-weight: 500;
  font-size: 1.0625rem;
  letter-spacing: 0.015em;
  line-height: 1.55;
  color: var(--glass-text-2);
}

.glass-card-actions {
  display: flex;
  gap: var(--glass-pad-sm);
  margin-top: var(--glass-pad-md);
}

/* Staggered entrance for multiple cards */
.glass-card:nth-child(1) { animation-delay: 0s; }
.glass-card:nth-child(2) { animation-delay: 0.15s; }
.glass-card:nth-child(3) { animation-delay: 0.3s; }
.glass-card:nth-child(4) { animation-delay: 0.45s; }
```

---

## Glass Button

Two variants: primary (accent glow) and ghost (transparent).

```css
.glass-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 10px 20px;
  font-family: inherit;
  font-weight: 600;
  font-size: 0.875rem;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  border-radius: var(--glass-radius-sm);
  cursor: pointer;
  transition: all var(--glass-duration-fast) var(--glass-ease-responsive);
  border: 1px solid transparent;
  position: relative;
  overflow: hidden;
}

.glass-btn-primary {
  background: rgba(255, 255, 255, 0.12);
  color: var(--glass-accent-cyan);
  border-color: rgba(160, 230, 245, 0.25);
}

.glass-btn-primary:hover {
  background: rgba(255, 255, 255, 0.18);
  border-color: rgba(160, 230, 245, 0.4);
  box-shadow: 0 0 20px var(--glass-glow-cyan);
}

.glass-btn-primary:active {
  background: rgba(255, 255, 255, 0.25);
  transform: scale(0.98);
}

.glass-btn-ghost {
  background: transparent;
  color: var(--glass-text-2);
  border-color: var(--glass-border);
}

.glass-btn-ghost:hover {
  background: rgba(255, 255, 255, 0.06);
  color: var(--glass-text-1);
  border-color: var(--glass-border-bright);
}

.glass-btn:focus-visible {
  outline: none;
  box-shadow: 0 0 0 2px rgba(160, 230, 245, 0.6), 0 0 24px rgba(160, 230, 245, 0.12);
}
```

---

## Glass Input

Text input fields with luminous bottom-border focus state.

```css
.glass-input-group {
  position: relative;
}

.glass-input-label {
  display: block;
  font-weight: 600;
  font-size: 0.75rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--glass-text-3);
  margin-bottom: 8px;
}

.glass-input {
  width: 100%;
  padding: 12px 16px;
  font-family: inherit;
  font-weight: 500;
  font-size: 1rem;
  letter-spacing: 0.02em;
  color: var(--glass-text-1);
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid var(--glass-border);
  border-radius: var(--glass-radius-sm);
  transition: all var(--glass-duration-fast) var(--glass-ease-responsive);
}

.glass-input::placeholder {
  color: var(--glass-text-3);
}

.glass-input:focus {
  outline: none;
  background: rgba(255, 255, 255, 0.07);
  border-color: rgba(160, 230, 245, 0.4);
  box-shadow: 0 1px 0 0 var(--glass-accent-cyan), 0 0 16px rgba(160, 230, 245, 0.08);
}
```

---

## Glass Notification

Enters with the slow, ambient 2-second materialization that respects peripheral vision.

```css
@keyframes notification-enter {
  0% {
    opacity: 0;
    transform: translateY(-8px) scale(0.98);
    filter: blur(8px);
  }
  60% {
    opacity: 0.7;
    filter: blur(2px);
  }
  100% {
    opacity: 1;
    transform: translateY(0) scale(1);
    filter: blur(0);
  }
}

@keyframes notification-exit {
  to {
    opacity: 0;
    transform: scale(0.98);
    filter: blur(4px);
  }
}

.glass-notification {
  position: fixed;
  top: 24px;
  right: 24px;
  max-width: 380px;
  padding: 16px 20px;
  background: var(--glass-surface-2);
  backdrop-filter: var(--glass-blur-heavy);
  -webkit-backdrop-filter: var(--glass-blur-heavy);
  border: 1px solid var(--glass-border);
  border-left: 3px solid var(--glass-accent-cyan);
  border-radius: var(--glass-radius-md);
  box-shadow: var(--glass-shadow-elevated);
  animation: notification-enter 1.8s var(--glass-ease-ambient) forwards;
  z-index: 1000;
}

.glass-notification.exiting {
  animation: notification-exit 0.6s var(--glass-ease-ambient) forwards;
}

.glass-notification-title {
  font-weight: 600;
  font-size: 0.9375rem;
  letter-spacing: 0.02em;
  color: var(--glass-text-1);
}

.glass-notification-body {
  font-weight: 500;
  font-size: 0.875rem;
  color: var(--glass-text-2);
  margin-top: 4px;
  line-height: 1.45;
}
```

---

## Glass Navigation Bar

A floating bottom nav that feels like a HUD element.

```css
.glass-navbar {
  position: fixed;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 4px;
  padding: 6px;
  background: var(--glass-surface-2);
  backdrop-filter: var(--glass-blur-heavy);
  -webkit-backdrop-filter: var(--glass-blur-heavy);
  border: 1px solid var(--glass-border);
  border-radius: 999px;
  box-shadow: var(--glass-shadow-elevated);
  z-index: 100;
}

.glass-nav-item {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 18px;
  font-weight: 600;
  font-size: 0.8125rem;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--glass-text-3);
  background: transparent;
  border: none;
  border-radius: 999px;
  cursor: pointer;
  transition: all var(--glass-duration-fast) var(--glass-ease-responsive);
}

.glass-nav-item:hover {
  color: var(--glass-text-2);
  background: rgba(255, 255, 255, 0.05);
}

.glass-nav-item.active {
  color: var(--glass-accent-cyan);
  background: rgba(160, 230, 245, 0.08);
}
```

---

## Glass Status Indicator

Pulsing dots and status labels for live data.

```css
.glass-status {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-weight: 600;
  font-size: 0.6875rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.glass-status-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  animation: glass-pulse 2s ease-in-out infinite;
}

.glass-status--active .glass-status-dot { background: var(--glass-accent-mint); }
.glass-status--active { color: var(--glass-accent-mint); }

.glass-status--warning .glass-status-dot { background: var(--glass-accent-gold); }
.glass-status--warning { color: var(--glass-accent-gold); }

.glass-status--error .glass-status-dot { background: var(--glass-accent-coral); }
.glass-status--error { color: var(--glass-accent-coral); }
```

---

## Glass Modal

Full-screen overlay with backdrop dim and centered glass panel.

```css
.glass-modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(8px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2000;
  animation: glass-materialize 0.6s var(--glass-ease-ambient) forwards;
}

.glass-modal {
  width: 90%;
  max-width: 520px;
  padding: var(--glass-pad-lg);
  background: var(--glass-surface-2);
  backdrop-filter: var(--glass-blur-heavy);
  border: 1px solid var(--glass-border);
  border-top-color: var(--glass-border-bright);
  border-radius: var(--glass-radius-lg);
  box-shadow: var(--glass-shadow-elevated);
  animation: glass-materialize 0.8s var(--glass-ease-ambient) 0.1s both;
}
```

---

## Glass Data Visualization

Styling for charts and data readouts that feel like HUD telemetry.

```css
.glass-data-label {
  font-weight: 600;
  font-size: 0.6875rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--glass-text-3);
}

.glass-data-value {
  font-weight: 700;
  font-size: 2.5rem;
  letter-spacing: -0.02em;
  color: var(--glass-text-1);
  font-variant-numeric: tabular-nums;
}

.glass-data-unit {
  font-weight: 500;
  font-size: 1rem;
  color: var(--glass-text-3);
  margin-left: 4px;
}

.glass-data-bar {
  height: 4px;
  background: rgba(255, 255, 255, 0.06);
  border-radius: 2px;
  overflow: hidden;
  margin-top: 8px;
}

.glass-data-bar-fill {
  height: 100%;
  border-radius: 2px;
  background: linear-gradient(90deg, var(--glass-accent-cyan), var(--glass-accent-lilac));
  transition: width 1s var(--glass-ease-ambient);
  box-shadow: 0 0 8px var(--glass-glow-cyan);
}

/* Circular gauge */
.glass-gauge {
  position: relative;
  width: 120px;
  height: 120px;
}

.glass-gauge svg {
  transform: rotate(-90deg);
}

.glass-gauge-track {
  fill: none;
  stroke: rgba(255, 255, 255, 0.06);
  stroke-width: 4;
}

.glass-gauge-fill {
  fill: none;
  stroke: var(--glass-accent-cyan);
  stroke-width: 4;
  stroke-linecap: round;
  stroke-dasharray: 339.292;
  stroke-dashoffset: 339.292;
  transition: stroke-dashoffset 1.5s var(--glass-ease-ambient);
  filter: drop-shadow(0 0 6px var(--glass-glow-cyan));
}
```

---

## HUD Reticle

Decorative corner brackets, crosshairs, and scanning lines for sci-fi feel.

```css
/* Corner brackets — add to a container to get HUD reticle corners */
.glass-hud-corners {
  position: relative;
}

.glass-hud-corners::before,
.glass-hud-corners::after,
.glass-hud-corners > .hud-bl::before,
.glass-hud-corners > .hud-br::before {
  content: '';
  position: absolute;
  width: 20px;
  height: 20px;
  border-color: rgba(160, 230, 245, 0.4);
  border-style: solid;
  border-width: 0;
}

.glass-hud-corners::before {
  top: 0; left: 0;
  border-top-width: 2px;
  border-left-width: 2px;
}

.glass-hud-corners::after {
  top: 0; right: 0;
  border-top-width: 2px;
  border-right-width: 2px;
}

/* Scan line animation */
@keyframes hud-scan {
  0% { top: 0; opacity: 0; }
  10% { opacity: 0.6; }
  90% { opacity: 0.6; }
  100% { top: 100%; opacity: 0; }
}

.glass-hud-scanline::after {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--glass-accent-cyan), transparent);
  animation: hud-scan 4s ease-in-out infinite;
  pointer-events: none;
}
```

---

## Glass Chip

Small label/tag elements.

```css
.glass-chip {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 6px 14px;
  font-weight: 600;
  font-size: 0.75rem;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  color: var(--glass-text-2);
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid var(--glass-border);
  border-radius: 999px;
}

.glass-chip--accent {
  color: var(--glass-accent-cyan);
  border-color: rgba(160, 230, 245, 0.2);
  background: rgba(160, 230, 245, 0.06);
}
```

---

## Glass Progress / Loading

```css
.glass-progress {
  width: 100%;
  height: 3px;
  background: rgba(255, 255, 255, 0.06);
  border-radius: 2px;
  overflow: hidden;
}

@keyframes glass-progress-indeterminate {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(300%); }
}

.glass-progress-bar {
  width: 30%;
  height: 100%;
  background: linear-gradient(90deg, transparent, var(--glass-accent-cyan), transparent);
  border-radius: 2px;
  animation: glass-progress-indeterminate 2s var(--glass-ease-ambient) infinite;
}

/* Determinate variant */
.glass-progress-bar--determinate {
  animation: none;
  background: var(--glass-accent-cyan);
  box-shadow: 0 0 8px var(--glass-glow-cyan);
  transition: width 0.6s var(--glass-ease-ambient);
}
```
