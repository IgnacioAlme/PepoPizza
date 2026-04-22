# AGENTS.md — AI Agent Guidelines for PepoPizza

This file provides guidelines for AI coding agents operating in this repository. Read before making changes.

---

## Project Overview

- **Type**: Static Astro website for a frozen pizza business
- **Stack**: Astro + Vanilla JS + CSS (no UI frameworks)
- **Output**: Static HTML deployed to Vercel/Netlify
- **Pages**: Landing (`/`), Menu (`/menu`), Instructions (`/instrucciones`)

---

## Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Start dev server at http://localhost:4321 |
| `npm run build` | Build for production to `dist/` |
| `npm run preview` | Preview production build locally |

**No test or lint commands exist.** Run `astro check` for TypeScript validation if needed.

---

## Tech Stack Conventions

### File Extensions
- **Components**: `.astro` files in `src/components/`
- **Pages**: `.astro` files in `src/pages/`
- **Layouts**: `.astro` files in `src/layouts/`
- **Styles**: CSS in `src/styles/global.css` + component-scoped `<style>`
- **Data**: `menu.json` at project root (single source of truth)

### Astro Patterns
- Import data directly: `import menu from '../../menu.json'`
- Client JS: Use `<script>` tags inside `.astro` files
- Inline data to JS: Use `define:vars={{ variable }}` or `define:const`
- No external `.js` files — all client logic goes in `<script>` blocks

---

## Code Style

### General
- **Language**: Spanish for UI text, English for code/IDs
- **IDs**: kebab-case (e.g., `pizza-muzzarella`, `8-porciones`)
- **CSS**: Use CSS custom properties from `global.css`
- **Images**: WebP format only, place in `/public/assets/images/`
- **No localStorage/cookies**: Cart is in-memory only

### Imports & Paths
```
# Relative paths from src/
import Layout from '../layouts/Layout.astro';
import menu from '../../menu.json';
```

### CSS Conventions
- Use existing CSS variables: `--color-primary`, `--color-surface`, `--radius-md`, etc.
- Scoped styles in each component via `<style>`
- Mobile-first: default styles for mobile, `@media (min-width: 768px)` for tablet+

### Naming
| Element | Convention | Example |
|---------|------------|---------|
| Components | kebab-case | `Hero.astro`, `Cart.astro` |
| Menu items | kebab-case | `pizza-muzzarella` |
| Variants | kebab-case | `8-porciones` |
| CSS classes | kebab-case | `.hero-container`, `.cart-sidebar` |
| Data IDs | kebab-case | `id: "pizzas"` |

### Functions & Variables
- **Functions**: camelCase, verb prefix (e.g., `addToCart`, `removeFromCart`, `updateCartUI`)
- **State**: simple variables for in-component state (e.g., `let cart = []`)
- **Window exposes**: Use `window.functionName = functionName` to make available globally

### Error Handling
- Silent failures for non-critical operations
- Log to console for debugging: `console.log('action: item added', itemKey)`
- No try/catch unless absolutely necessary
- Never expose internal errors to users

---

## Menu Data Structure

All menu data comes from `menu.json`. Never hardcode items.

```json
{
  "business": { "name": "...", "whatsapp": "54911..." },
  "categories": [
    {
      "id": "pizzas",
      "label": "Pizzas Artesanales",
      "items": [
        {
          "id": "muzzarella",
          "name": "Muzzarella",
          "description": "...",
          "price": 8000,
          "image": "/assets/images/items/muzzarella.webp",
          "available": true
        }
      ]
    }
  ],
  "variants": [...],
  "values": [...],
  "convenience": [...]
}
```

**Rules:**
- `price` is always an integer in ARS (no currency symbol)
- `available: false` hides item from UI
- Image paths are relative to `/public/`

---

## Cart Logic

- **State**: In-memory array `cart = []`
- **Item shape**: `{ id, name, price, variant, variantLabel, quantity, itemKey }`
- **Functions**:
  - `addToCart(id, name, price, variant, variantLabel)` — creates item key from `${id}-${variant}`
  - `removeFromCart(itemKey)` — decrements quantity, removes if 0
  - `updateCartUI()` — renders cart items and totals

- **WhatsApp redirect** (from Cart.astro):
  ```js
  const url = `https://wa.me/${number}?text=${encodeURIComponent(message)}`;
  window.open(url, '_blank');
  ```

---

## UI Patterns

### Buttons
- Primary: `.btn-primary` (tomato red)
- Secondary: `.btn-secondary` (outlined)
- Add to cart: `.btn-add`, `.btn-add-to-cart`, `.btn-add-variant` with `data-*` attributes

### Sections
- Hero (`#hero`)
- Owner Story (`#historia`)
- Menu (`#menu`)
- Footer (`#footer`)

### Responsive
```css
/* Mobile first */
.hero { padding: 2rem 0; }
@media (min-width: 768px) { .hero { padding: 4rem 0; } }
```

---

## What NOT To Do

- **Do not** add backend, database, or API routes
- **Do not** add authentication or user accounts
- **Do not** use localStorage or sessionStorage
- **Do not** add payment processing
- **Do not** hardcode menu items — always source from `menu.json`
- **Do not** add heavy UI libraries (MUI, Chakra, etc.)
- **Do not** add new dependencies without user approval

---

## Accessibilty

- All images need `alt` text
- Buttons need `aria-label` when icon-only
- Focus states on interactive elements
- Color contrast ≥ 4.5:1

---

*Last updated: April 2026*