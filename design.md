# POWER-UP: Official Visual Direction & Design System

## 1. Aesthetic Philosophy & Brand Identity
The POWER-UP brand visual direction is heavily influenced by retro 80s sports and lifestyle brands, drawing explicit inspiration from the raw, high-energy aesthetics of early Kodak and Nike campaigns.

The brand relies on an aesthetic of **"Speed, Calm, and Direct Action."** It avoids over-explanation, opting instead for a simple, straight-to-the-point visual language that speaks to an audience that "doesn't have time to listen."

**Core Visual Cues:**
- Flash photography, polaroid, and fisheye lens-style imagery.
- Focus on movement, lifestyle, and high energy.
- Uninterrupted day-or-night continuity.
- Grainy, warm gradients and heavy blurs.

---

## 2. Official Color Palette
The palette deliberately avoids being overly colorful while utilizing warm, high-contrast tones that build emotional trust and easy recognition. It is gender-neutral, modern, and design-driven.

### Core Hex Codes (CSS Variables)
- **Chili Pepper:** `#FF290D` (RGB: 255, 41, 13) — *Primary Action / High Energy*
- **Lime Juice:** `#E8FCAC` (RGB: 232, 252, 172) — *High-Contrast Accents*
- **Honey Comb:** `#F5AB18` (RGB: 245, 171, 24) — *Warmth & Accessibility*
- **Olive:** `#4A4918` (RGB: 74, 73, 24) — *Earthy Mid-Tones*
- **Charcoal:** `#131313` (RGB: 18, 18, 18) — *Absolute Backgrounds / Void Space*

---

## 3. Typography
The typographic hierarchy is built on bold, commanding sans-serif or serif variations that feel confident and timeless.
- **Header:** Used as a strong, bold statement. High impact.
- **Subheader:** Used for secondary information and contextual bridging.
- **Body Copy:** Functional, readable, default text.

*(Note: Implementation should map these to a bold, retro-modern sans-serif stack like `Outfit` or a tightly-tracked serif, avoiding overly delicate weights).*

---

## 4. Gradients & Textures (The "Grain" Effect)
Instead of flat color blocking or clean digital gradients, the brand relies on **textured warmth**. 
- Gradients blend Chili Pepper, Honey Comb, and Lime Juice.
- **Grain Overlay:** A heavy film-grain noise effect is applied across gradients to maintain consistency with the retro 80s flash-photography look and feel. 

*CSS Implementation Note: This requires SVG noise filters or `mix-blend-mode: multiply` with a noise pattern overlay across background elements.*

---

## 5. Logo Architecture
The primary POWER-UP logo utilizes a staggered, "glitch" or slanted motion effect, reflecting the concepts of speed and constant access.
- It must be used in its approved colorways (primarily Charcoal on Lime Juice, or Chili Pepper on Charcoal).
- The logo icon acts as a dynamic mark for small-format screens and motion sequences.

---

## 6. Frontend CSS Translation (Target Architecture)
To implement this official design system into the web application, the CSS architecture must reflect the following tokens:

```css
:root {
  --chili-pepper: #FF290D;
  --lime-juice: #E8FCAC;
  --honey-comb: #F5AB18;
  --olive: #4A4918;
  --charcoal: #131313;
  
  --background: var(--charcoal);
  --foreground: var(--lime-juice);
  --accent: var(--chili-pepper);
  --brand-warn: var(--honey-comb);
}

/* Grain Filter Overlay */
.grain-overlay {
  content: "";
  position: fixed;
  inset: 0;
  pointer-events: none;
  background-image: url('/assets/noise.png'); /* Or SVG filter */
  opacity: 0.15;
  mix-blend-mode: overlay;
  z-index: 9999;
}
```

The application must shed generic "glassmorphism" in favor of solid retro blocking, high-contrast typography, and intense, grainy color fields that embody the "momentum-driven life."
