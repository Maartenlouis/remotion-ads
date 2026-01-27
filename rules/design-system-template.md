---
title: Visual Design System Template
description: Template for configuring brand colors, fonts, and assets
section: setup
priority: high
tags: [branding, colors, fonts, design-system, configuration]
---

# Visual Design System Template

Copy this file to `design-system.md` and fill in your brand values.

---

## Brand Colors

Replace placeholder values with your brand colors:

```tsx
const COLORS = {
  // Primary colors
  primary: "#000000",           // TODO: Your main brand color
  primaryLight: "#333333",      // TODO: Lighter variant
  primaryDark: "#111111",       // TODO: Darker variant

  // Secondary/accent
  secondary: "#666666",         // TODO: Secondary brand color
  accent: "#888888",            // TODO: Highlight/accent color

  // Backgrounds
  background: "#ffffff",        // TODO: Light background
  backgroundDark: "#f5f5f5",    // TODO: Darker background variant

  // Text
  foreground: "#1a1a1a",        // TODO: Primary text color
  muted: "#888888",             // TODO: Muted/secondary text

  // Status (optional)
  success: "#22c55e",
  warning: "#f59e0b",
  error: "#ef4444",

  // Special (optional)
  orange: "#f97316",            // For star ratings
};
```

### Color Usage Guide

| Color | Use For |
|-------|---------|
| `primary` | Headers, buttons, key elements |
| `primaryLight` | Hover states, accents |
| `primaryDark` | Backgrounds for dark scenes |
| `secondary` | Supporting elements, icons |
| `accent` | Highlights, active states |
| `background` | Light scene backgrounds |
| `foreground` | Main body text |
| `muted` | Secondary text, hints |

---

## Typography

### Font Configuration

```tsx
// TODO: Replace with your brand fonts
// Browse available fonts: https://www.remotion.dev/docs/google-fonts
import { loadFont } from "@remotion/google-fonts/YourHeadingFont";
import { loadFont as loadBodyFont } from "@remotion/google-fonts/YourBodyFont";

const { fontFamily: headingFont } = loadFont();
const { fontFamily: bodyFont } = loadBodyFont();
```

### Recommended Font Pairings

| Style | Heading Font | Body Font |
|-------|--------------|-----------|
| Modern/Bold | Anton, Bebas Neue | Plus Jakarta Sans, Inter |
| Professional | Montserrat, Raleway | Open Sans, Lato |
| Elegant | Playfair Display | Source Sans Pro |
| Tech | Space Grotesk | IBM Plex Sans |
| Casual | Poppins | Nunito |

### Font Sizes

| Element | Reels (1920h) | Carousel (1350h) | Weight |
|---------|---------------|------------------|--------|
| Hero headline | 64-80px | 58px | 700 |
| Section headline | 52-64px | 48px | 600-700 |
| Body text | 44-52px | 34-40px | 500 |
| Bullet points | 40-48px | 30-34px | 500-600 |
| Caption/note | 36-40px | 24-28px | 400 |
| CTA button | 48-56px | 36px | 600-700 |

---

## Logo & Branding

### Logo Path

```tsx
// TODO: Update with your logo path
const LOGO_PATH = "your-logo.png";  // In public/ folder
```

### Logo Sizing

| Context | Size | Notes |
|---------|------|-------|
| CTA slide (prominent) | 300-400px height | Centered, prominent |
| Corner badge | 80-120px | Top corner watermark |
| Inline with text | 48-64px | Next to brand name |

### Brand Name Display

```tsx
// TODO: Your brand name
const BRAND_NAME = "Your Brand";
const BRAND_TAGLINE = "Your tagline here";
```

---

## Icon System

### Icon Paths

```tsx
// TODO: Define your icon library paths
const ICONS = {
  // Example structure - replace with your icons
  checkmark: "images/instagram-ads/icons/checkmark.svg",
  warning: "images/instagram-ads/icons/warning.svg",
  arrow: "images/instagram-ads/icons/arrow.svg",
  // Add more as needed
};
```

### 3D/Illustrated Icons (Optional)

If using AI-generated or illustrated icons:

```tsx
const ILLUSTRATED_ICONS = {
  // Example: 3D clay-style icons
  hero: "images/instagram-ads/illustrations/hero-icon.png",
  problem1: "images/instagram-ads/illustrations/problem1.png",
  problem2: "images/instagram-ads/illustrations/problem2.png",
  solution: "images/instagram-ads/illustrations/solution.png",
  cta: "images/instagram-ads/illustrations/cta-icon.png",
};
```

### Icon Sizing by Context

| Context | Size | Notes |
|---------|------|-------|
| Hero/Hook (Scene 1) | 160-240px | Large, attention-grabbing |
| Problem list items | 55-75px | Inline with text |
| Feature highlight | 60-100px | Medium emphasis |
| Solution/CTA | 140-180px | Large, prominent |
| Decorative | 32-48px | Background accents |

---

## Background Patterns

### Available Patterns

After running `scripts/generate-backgrounds.js`:

| File | Pattern | Best For |
|------|---------|----------|
| `dots.png` | Regular dot grid | General slides |
| `gradient.png` | Vertical gradient | Subtle depth |
| `solid-primary.png` | Solid brand color | CTA slides |
| `grain.png` | Noise texture | Organic feel |

### Custom Patterns

Add more patterns by editing `scripts/generate-backgrounds.js`.

---

## Animation Settings

### Spring Configurations

```tsx
// Smooth (no bounce) - professional feel
const SPRING_SMOOTH = { damping: 200 };

// Bouncy - playful, attention-grabbing
const SPRING_BOUNCY = { damping: 8, stiffness: 200 };

// Quick - snappy transitions
const SPRING_QUICK = { damping: 15, stiffness: 100 };
```

### Standard Animations

```tsx
// Fade in with slide up
const FadeInText: React.FC<{ children: React.ReactNode; delay?: number }> = ({
  children,
  delay = 0
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const progress = spring({
    frame: frame - delay,
    fps,
    config: SPRING_SMOOTH,
  });

  return (
    <div style={{
      opacity: interpolate(progress, [0, 1], [0, 1]),
      transform: `translateY(${interpolate(progress, [0, 1], [30, 0])}px)`,
    }}>
      {children}
    </div>
  );
};
```

---

## Grainy Overlay

For texture on all slides:

```tsx
const GrainyOverlay: React.FC<{ opacity?: number }> = ({ opacity = 0.05 }) => {
  const noiseFilter = `
    <svg xmlns="http://www.w3.org/2000/svg" width="300" height="300">
      <filter id="noise" x="0" y="0" width="100%" height="100%">
        <feTurbulence type="fractalNoise" baseFrequency="0.8" numOctaves="4" stitchTiles="stitch"/>
        <feColorMatrix type="saturate" values="0"/>
      </filter>
      <rect width="100%" height="100%" filter="url(#noise)" opacity="1"/>
    </svg>
  `;

  return (
    <div
      style={{
        position: "absolute",
        inset: 0,
        backgroundImage: `url("data:image/svg+xml,${encodeURIComponent(noiseFilter)}")`,
        backgroundRepeat: "repeat",
        opacity,
        mixBlendMode: "overlay",
        pointerEvents: "none",
      }}
    />
  );
};
```

**Opacity guide:**
- Dark backgrounds: `0.05-0.10`
- Light backgrounds: `0.03-0.05`

---

## Gradient Backgrounds

### Scene-specific gradients

```tsx
// Hook scene (dramatic)
const hookGradient = `linear-gradient(160deg, ${COLORS.primaryDark} 0%, ${COLORS.primary} 100%)`;

// Problem scene (serious)
const problemGradient = `linear-gradient(160deg, ${COLORS.primaryDark} 0%, #1a1a1a 100%)`;

// Solution scene (positive)
const solutionGradient = `linear-gradient(155deg, ${COLORS.primary} 0%, ${COLORS.primaryLight} 100%)`;

// CTA scene (brand focus)
const ctaGradient = `linear-gradient(180deg, ${COLORS.background} 0%, ${COLORS.backgroundDark} 100%)`;
```

---

## Complete Example Component

```tsx
import React from "react";
import {
  AbsoluteFill,
  Img,
  staticFile,
  useCurrentFrame,
  useVideoConfig,
  spring,
  interpolate,
} from "remotion";

// ============================================
// TODO: UPDATE THESE WITH YOUR BRAND VALUES
// ============================================
const COLORS = {
  primary: "#000000",
  secondary: "#666666",
  background: "#ffffff",
  foreground: "#1a1a1a",
  accent: "#888888",
};

// TODO: Replace with your brand fonts
import { loadFont } from "@remotion/google-fonts/YourHeadingFont";
import { loadFont as loadBodyFont } from "@remotion/google-fonts/YourBodyFont";

const { fontFamily: headingFont } = loadFont();
const { fontFamily: bodyFont } = loadBodyFont();

const SPRING_SMOOTH = { damping: 200 };
// ============================================

const FadeInText: React.FC<{ children: React.ReactNode; delay?: number; style?: React.CSSProperties }> = ({
  children,
  delay = 0,
  style = {},
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const progress = spring({
    frame: frame - delay,
    fps,
    config: SPRING_SMOOTH,
  });

  return (
    <div style={{
      opacity: interpolate(progress, [0, 1], [0, 1]),
      transform: `translateY(${interpolate(progress, [0, 1], [30, 0])}px)`,
      ...style,
    }}>
      {children}
    </div>
  );
};

const GrainyOverlay: React.FC<{ opacity?: number }> = ({ opacity = 0.05 }) => {
  const noiseFilter = `
    <svg xmlns="http://www.w3.org/2000/svg" width="300" height="300">
      <filter id="noise" x="0" y="0" width="100%" height="100%">
        <feTurbulence type="fractalNoise" baseFrequency="0.8" numOctaves="4" stitchTiles="stitch"/>
        <feColorMatrix type="saturate" values="0"/>
      </filter>
      <rect width="100%" height="100%" filter="url(#noise)" opacity="1"/>
    </svg>
  `;

  return (
    <div
      style={{
        position: "absolute",
        inset: 0,
        backgroundImage: `url("data:image/svg+xml,${encodeURIComponent(noiseFilter)}")`,
        backgroundRepeat: "repeat",
        opacity,
        mixBlendMode: "overlay",
        pointerEvents: "none",
      }}
    />
  );
};

const SafeContent: React.FC<{ children: React.ReactNode; style?: React.CSSProperties }> = ({
  children,
  style = {},
}) => (
  <div style={{
    position: "absolute",
    top: 285,
    bottom: 400,
    left: 80,
    right: 120,
    display: "flex",
    flexDirection: "column",
    alignItems: "center",
    justifyContent: "center",
    ...style,
  }}>
    {children}
  </div>
);

// Example Scene 1: Hook
export const Scene1Hook: React.FC = () => {
  const { fps } = useVideoConfig();

  return (
    <AbsoluteFill>
      <AbsoluteFill style={{
        background: `linear-gradient(160deg, ${COLORS.primary} 0%, #1a1a1a 100%)`,
      }} />
      <GrainyOverlay opacity={0.08} />

      <SafeContent>
        {/* TODO: Add your icon */}
        <FadeInText>
          <Img
            src={staticFile("images/instagram-ads/illustrations/your-hero-icon.png")}
            style={{ width: 180, height: 180, objectFit: "contain" }}
          />
        </FadeInText>

        <FadeInText delay={fps * 0.15} style={{
          fontFamily: headingFont,
          fontSize: 64,
          fontWeight: 700,
          color: COLORS.background,
          textAlign: "center",
          marginTop: 40,
          lineHeight: 1.15,
        }}>
          Your <span style={{ color: COLORS.accent }}>Hook</span>
          <br />
          Headline Here
        </FadeInText>

        <FadeInText delay={fps * 0.35} style={{
          fontFamily: bodyFont,
          fontSize: 48,
          color: COLORS.accent,
          textAlign: "center",
          marginTop: 24,
        }}>
          Supporting subtitle text.
        </FadeInText>
      </SafeContent>
    </AbsoluteFill>
  );
};
```

---

## Checklist

Before creating ads, ensure you've configured:

- [ ] Brand colors in `COLORS` object
- [ ] Heading and body fonts imported
- [ ] Logo path defined
- [ ] Icon paths defined (if using)
- [ ] Background patterns generated
- [ ] Animation spring configs set
- [ ] Grainy overlay opacity tuned for your colors
