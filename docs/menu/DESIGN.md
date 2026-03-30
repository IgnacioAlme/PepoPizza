# Design System Document

## 1. Overview & Creative North Star: "The Modern Artisanal Pantry"

This design system is built to transform a frozen pizza brand into a premium, editorial digital experience. We are moving away from the "industrial" aesthetic of frozen foods toward **"The Modern Artisanal Pantry."** This concept blends the warmth of a traditional Italian kitchen with the crisp, clean precision of modern digital design.

Our strategy breaks the standard grid-based "e-commerce template" look by utilizing:
- **Organic Asymmetry:** Drawing inspiration from the hand-tossed nature of pizza dough.
- **Editorial Layering:** Treating the screen like a high-end food magazine, where images and typography overlap to create depth.
- **Warmth vs. Frost:** A visual dialogue between the rich, appetizing primary reds (`primary`) and the cool, "frozen" accents (`tertiary`), all anchored by a soft, cream-based neutral palette.

---

## 2. Color Strategy: Tonal Sophistication

The palette is rooted in appetizing warmth, balanced by functional neutrals and a cooling secondary tone to represent the frozen category.

### Palette Application
- **Primary (`#b51a0a`):** Use for high-action items and key brand moments. It represents the rich tomato sauce and the brand's energy.
- **Secondary (`#8c4a00`):** Inspired by perfectly baked crust, use this for stabilizing elements and secondary actions.
- **Tertiary (`#2c5f87`):** The "Frost" accent. Use this sparingly for "Frozen-at-Peak-Freshness" callouts and temperature-related iconography.
- **Surface (`#fcf6e8`):** The "Warm Cream" foundation. This is our primary canvas, providing a softer, more premium feel than clinical white.

### The "No-Line" Rule
To maintain an editorial feel, **prohibit 1px solid borders for sectioning.** Boundaries must be defined solely through background color shifts. For example, a card should be defined by placing a `surface-container-lowest` (#ffffff) element on a `surface` (#fcf6e8) background.

### The "Glass & Gradient" Rule
Standard flat colors lack the "soul" required for food branding. 
- **Signature Gradients:** Use a subtle linear transition from `primary` to `primary-container` for Hero CTAs to give them a rounded, "3D" appetizing quality.
- **Frozen Glass:** For floating product overlays, use `surface-container-lowest` with a 60% opacity and a `20px` backdrop-blur to mimic the frosted glass of a freezer door.

---

## 3. Typography: The Playful Critic

The typography pairing reflects a balance between a bold, nostalgic pizza parlor and a modern, clean digital interface.

- **Display & Headlines (Epilogue):** This is our "bold script" alternative. Its high-contrast, geometric shapes provide a playful, rhythmic quality. Use `display-lg` (3.5rem) for hero statements and `headline-md` (1.75rem) for category titles.
- **Body & Titles (Plus Jakarta Sans):** A contemporary sans-serif that ensures high readability across small screens. 
- **The Hierarchy Strategy:** Headlines should always lead with `on-surface` (#312f26) to ensure authority, while body text uses `on-surface-variant` (#5f5b51) to create a softer reading experience that doesn't compete with the appetizing food photography.

---

## 4. Elevation & Depth: Tonal Layering

We reject traditional drop shadows in favor of a physical, "stacked paper" philosophy.

- **The Layering Principle:** Depth is achieved by "stacking" surface-container tiers. 
    - *Base:* `surface`
    - *Section:* `surface-container-low`
    - *Card/Interactive:* `surface-container-lowest`
- **Ambient Shadows:** If a floating element (like a shopping cart) requires a shadow, it must be ultra-diffused: `box-shadow: 0 12px 32px rgba(49, 47, 38, 0.05)`. Note the use of the `on-surface` color for the shadow tint rather than pure black.
- **The Ghost Border:** For accessibility in forms, use `outline-variant` (#b2ada0) at 20% opacity. Never use 100% opaque lines.

---

## 5. Components

### Buttons
- **Primary:** Rounded-full (`full`), `primary` background, `on-primary` text. Use a subtle inner-glow (top-down) to mimic a baked surface.
- **Secondary:** `surface-container-highest` background. No border. Soft and integrated.
- **States:** On hover, shift background to `primary-dim`. Use a `0.2s` ease-in-out transition.

### Cards (The "Fresh-Box" Style)
- Forbid divider lines. Separate "Ingredients" from "Cooking Instructions" using a `spacing-8` (2rem) vertical gap or a subtle shift from `surface` to `surface-container-low`.
- **Roundedness:** Use `xl` (1.5rem) for top-level product cards to mimic the circular nature of the pizza. Use `md` (0.75rem) for smaller UI elements like tooltips.

### Iconography: The "Ice-to-Oven" Set
Icons should be thin-line (`1.5px`) but with rounded terminals.
- **Ingredient Icons:** Minimalist toppings (pepperoni, basil leaf).
- **Frozen Icons:** Use the `tertiary` (#2c5f87) color for "ice crystal" or "sub-zero" indicators.
- **Appetite Icons:** Use `primary` (#b51a0a) for heat/flame icons.

### Input Fields
- Background: `surface-container-low`.
- Active State: A `2px` "Ghost Border" using `primary` at 40% opacity. 

---

## 6. Do's and Don'ts

### Do
- **Do** overlap product imagery over container edges to break the "boxed" feel.
- **Do** use the `spacing-24` (6rem) scale for hero sections to create a sense of luxury and "breathing room."
- **Do** use the `full` roundedness for "Selection Chips" to make them feel like dough-balls or toppings.

### Don't
- **Don't** use pure black (#000000) for text. Use `on-background` (#312f26) to keep the warmth.
- **Don't** use standard 1px dividers. Use a `surface-variant` background block or whitespace.
- **Don't** use harsh, high-contrast shadows. If it looks "pasted on," the shadow is too dark.
- **Don't** crowd the UI. The "Artisanal" feel relies on the quality of whitespace as much as the quality of the ingredients.