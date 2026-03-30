# AGENT.md — Pizza Place Landing Page & Light Ecommerce

This file is intended for AI agents working on this project. Read it fully before taking any action. It defines what this project is, what it must do, what it must NOT do, the technical conventions to follow, and how to handle common tasks.

---

## Project Summary

A **single-page static website** for a pizza restaurant. The site presents business information and a digital menu. Users can add items to a cart, see the running total, and place an order by being redirected to WhatsApp with a pre-filled message template containing their selected items and total cost.

There is **no backend, no database, no authentication, and no payment processing.** All state is client-side only.

---

## Primary Goals

1. Show business info: name, description, hours, address, social links.
2. Display a categorized menu with item names, descriptions, prices, and photos.
3. Let users add/remove items from a cart with live total cost updates.
4. On checkout, redirect to a WhatsApp chat with a pre-formatted order message.
5. Work well on mobile (primary audience) and desktop.

---

## Tech Stack

| Layer | Choice | Notes |
|---|---|---|
| Framework | **Astro** | Static site generator, use `.astro` components |
| Language | HTML5 + CSS3 + Vanilla JS | Use Astro components with client-side scripts |
| Styling | CSS custom properties + BEM | No UI component libraries (keep it lightweight) |
| Data | `menu.json` flat file | Single source of truth for all menu items |
| Hosting | Vercel or Netlify (free tier) | Static deploy, no server-side rendering needed |
| Images | WebP format, locally hosted in `/public/assets/images/` | Optimize all images before committing |
| WhatsApp | `wa.me` click-to-chat API | No WhatsApp Business API required |

---

## File Structure

```
/
├── AGENT.md                  ← this file
├── index.html                ← entry point (or pages/index.jsx for Next.js)
├── menu.json                 ← ALL menu data lives here
├── assets/
│   ├── images/
│   │   ├── hero.webp
│   │   └── items/            ← one image per menu item, named by item id
│   └── icons/
├── styles/
│   └── main.css
└── scripts/
    ├── cart.js               ← cart state, add/remove, total calculation
    └── whatsapp.js           ← message generation and redirect logic
```

For a **React/Next.js** version, the equivalent structure is:
```
/
├── AGENT.md
├── menu.json
├── public/assets/
├── src/
│   ├── components/
│   │   ├── Hero.jsx
│   │   ├── AboutSection.jsx
│   │   ├── MenuSection.jsx
│   │   ├── MenuCard.jsx
│   │   ├── Cart.jsx
│   │   └── Footer.jsx
│   ├── hooks/
│   │   └── useCart.js        ← cart logic as a custom hook
│   ├── utils/
│   │   └── whatsapp.js
│   └── pages/index.jsx
```

---

## Astro File Structure

```
/
├── AGENT.md
├── menu.json                    ← ALL menu data
├── astro.config.mjs
├── package.json
├── public/
│   ├── assets/
│   │   └── images/
│   │       ├── hero.webp
│   │       └── items/           ← one image per menu item
│   └── favicon.svg
└── src/
    ├── layouts/
    │   └── Layout.astro         ← base HTML layout
    ├── components/
    │   ├── Hero.astro
    │   ├── About.astro
    │   ├── MenuSection.astro
    │   ├── MenuCard.astro
    │   ├── Cart.astro
    │   └── Footer.astro
    ├── styles/
    │   └── global.css
    └── pages/
        └── index.astro          ← entry point
```

### Astro-Specific Guidelines

- **Components**: All UI sections are `.astro` components
- **Client-side interactivity**: Use `<script>` tags inside `.astro` files for cart logic
- **Reactivity**: Use Astro Signals (`@astrojs/signals`) for cart state, or vanilla JS with custom events
- **Data import**: Import `menu.json` directly in Astro components with `import menu from '../menu.json'`
- **Styling**: Use `<style>` blocks in each component, import `global.css` in Layout

---

## How to Close Issues

When you complete a task (issue), **close the issue yourself** to mark it as done:

```bash
gh issue close <issue-number>
```

For example, after completing issue #3:
```bash
gh issue close 3
```

This keeps the project board clean and shows progress. Each issue description includes the close command.

---

## menu.json Schema

Every menu item must conform to this structure. Do not invent new fields without updating this spec.

```json
{
  "business": {
    "name": "PepoPizza",
    "tagline": "Las mejores pizzas de la ciudad",
    "description": "Descripción breve del negocio...",
    "address": "Dirección del local",
    "hours": "Jue a Dom: 19:00 - 23:30",
    "whatsapp": "5493624XXXXXX"
  },
  "categories": [
    {
      "id": "pizzas",
      "label": "Pizzas",
      "items": [
        {
          "id": "pizza-muzzarella",
          "name": "Pizza Muzzarella",
          "description": "Salsa de tomate y mozzarella.",
          "price": 8500,
          "image": "/assets/images/items/pizza-muzzarella.webp",
          "available": true
        }
      ]
    }
  ]
}
```

**Rules:**
- `id` must be unique across ALL items and categories, kebab-case.
- `price` is always an integer in ARS (Argentine Pesos), no currency symbol.
- `available: false` hides the item from the menu without deleting it.
- Image paths are relative to the project root.
- Never hardcode menu data in component or HTML files — always read from `menu.json`.

---

## Cart Logic

The cart is **ephemeral client-side state only** — it resets on page reload. No localStorage, no cookies.

### Data shape

```javascript
// cart is an array of entries
[
  { itemId: "pizza-muzzarella", name: "Pizza Muzzarella", price: 8500, quantity: 2 },
  { itemId: "coca-cola-1l",     name: "Coca-Cola 1L",     price: 3200, quantity: 1 }
]
```

### Required functions (cart.js or useCart.js)

| Function | Behavior |
|---|---|
| `addItem(item)` | If item exists in cart, increment quantity. Otherwise push new entry with quantity 1. |
| `removeItem(itemId)` | Decrement quantity. If quantity reaches 0, remove from cart entirely. |
| `clearCart()` | Empty the cart array. |
| `getTotal()` | Return `sum of (price × quantity)` for all entries. |
| `getCount()` | Return total number of individual items (sum of quantities). |

---

## WhatsApp Integration

### Number format

The WhatsApp number must be stored as a constant in `whatsapp.js`:

```javascript
const WHATSAPP_NUMBER = "5493624XXXXXX"; // country code + area code + number, no spaces or +
```

### Message format

Generate the message with this exact template:

```
¡Hola! Quisiera hacer el siguiente pedido:

🍕 {item name} x{quantity} — ${price formatted}
...

💰 *Total: ${total formatted}*

¿Pueden confirmarlo? ¡Gracias!
```

- Format prices with `.toLocaleString('es-AR')` for thousand separators.
- Use the pizza emoji 🍕 for food items, 🥤 for drinks, 🍰 for desserts. Default to 🍽️ if category is unknown.
- Bold the total line using WhatsApp markdown (`*text*`).

### Redirect

```javascript
function redirectToWhatsApp(cart) {
  const message = buildMessage(cart);
  const url = `https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`;
  window.open(url, "_blank");
}
```

Always open in a new tab (`_blank`). Never navigate away from the page.

---

## Page Sections

The page is a **single scrollable document** with these sections, in order:

| Section | ID | Content |
|---|---|---|
| Hero | `#hero` | Logo, business name, tagline, CTA button scrolling to `#menu` |
| About | `#about` | Business description, opening hours, address, optional Google Maps embed |
| Menu | `#menu` | Category tabs/filters, item cards with add-to-cart buttons |
| Footer | `#footer` | WhatsApp link, social media links, address, hours |

The **Cart** is a floating element (fixed position), always visible, showing item count badge and total. It expands into a sidebar/drawer on interaction.

---

## UI & Design Conventions

### Colors (CSS custom properties)

```css
:root {
  --color-primary:    #C0392B; /* tomato red — buttons, highlights */
  --color-secondary:  #F39C12; /* golden orange — prices, badges */
  --color-bg:         #1A1A1A; /* dark background (or #FAFAF7 for light theme) */
  --color-text:       #FFFFFF; /* main text on dark bg */
  --color-whatsapp:   #25D366; /* WhatsApp green — checkout button ONLY */
  --color-muted:      #888888; /* secondary text, descriptions */
}
```

### Typography

- Headings: a display/serif font with personality (Playfair Display, Oswald, or similar from Google Fonts).
- Body/prices: a clean sans-serif (Lato, Source Sans 3, or similar).
- Never use Inter, Roboto, or Arial.

### Responsive breakpoints

```css
/* Mobile first */
/* Default styles target mobile (< 768px) */
@media (min-width: 768px)  { /* tablet  */ }
@media (min-width: 1024px) { /* desktop */ }
```

### Accessibility minimum requirements

- All `<img>` tags must have a descriptive `alt` attribute.
- Interactive elements (buttons, links) must have visible focus styles.
- Color contrast ratio must be ≥ 4.5:1 for normal text.
- The "Order via WhatsApp" button must have an `aria-label` describing its action.

---

## What Agents Must NOT Do

- **Do not add a backend.** No Node.js server, no API routes, no database, no CMS.
- **Do not implement payment processing** of any kind.
- **Do not add user authentication** or accounts.
- **Do not use localStorage or sessionStorage.** Cart state is in-memory only.
- **Do not hardcode menu items** in HTML or JSX. Always source from `menu.json`.
- **Do not use heavy UI libraries** (MUI, Ant Design, Chakra, etc.). Keep the bundle lean.
- **Do not change the WhatsApp message format** without updating this document and the template in `whatsapp.js`.
- **Do not add new dependencies** without checking if the functionality can be achieved with what's already available.

---

## Definition of Done

A feature or task is considered complete when:

- [ ] It works correctly on mobile (375px width minimum).
- [ ] It works correctly on desktop (1280px width).
- [ ] It has been tested in Chrome and Firefox.
- [ ] No console errors or warnings.
- [ ] Images are in WebP format and under 200KB each.
- [ ] Menu data is sourced exclusively from `menu.json`.
- [ ] The WhatsApp redirect produces a correctly formatted, readable message.
- [ ] The cart total updates correctly when items are added or removed.

---

## Out of Scope (Future Phases)

These features are explicitly NOT part of this project. Do not implement them unless the project scope is officially updated:

- Admin panel for menu management
- MercadoPago or any payment gateway integration
- Table reservation system
- Order tracking
- Push notifications
- User accounts or login

---

## Key Business Rules

- All prices are in **Argentine Pesos (ARS)**.
- The WhatsApp number receiving orders must be configurable from a single constant — never scattered across the codebase.
- If an item has `available: false` in `menu.json`, it must not be shown to users and must not be addable to the cart.
- The cart must show a running total at all times while it contains at least one item.
- The checkout button (WhatsApp redirect) must only be enabled/visible when the cart has at least one item.

---

*Last updated: March 2026. If you modify a core convention described in this file, update this file in the same commit/change.*
