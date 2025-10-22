# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a restaurant mockup website ("L'Attellu") - a static HTML/CSS/JavaScript project for a French gastronomic restaurant. The site features a landing page and an online ordering system, both built as self-contained HTML files with embedded styles and scripts.

**Tech Stack:** Pure HTML5, CSS3 (with CSS variables), and vanilla JavaScript - no frameworks or build tools.

**Target Environment:** WAMP64 server on Windows (located at `C:\wamp64\www\restomaquette`).

## File Structure

- **index.html** - Main landing page with hero section, about, gallery, menu display, and contact information
- **commande.html** - Online ordering page with interactive cart, menu browsing, and checkout flow

Both files are completely self-contained with all CSS in `<style>` tags and all JavaScript in `<script>` tags.

## Architecture & Design Patterns

### CSS Architecture
- Uses CSS custom properties (variables) defined in `:root` for theming:
  - `--primary-color: #000000` (black)
  - `--accent-color: #dc143c` (crimson red - used for brand name "L'Attellu")
  - `--success-color`, `--danger-color`, `--text-light`, `--text-dark`, `--bg-light`, `--shadow`
- All styling uses these variables for consistency
- Brand name "L'Attellu" is always displayed in red (accent color)
- Responsive design with mobile breakpoints at 768px and 480px

### JavaScript Architecture (commande.html)

**Data Management:**
- Menu items stored in `menuData` object, organized by category (entrees, plats, desserts, boissons)
- Each item has: id, name, description, price, icon (SVG), optional badge
- Cart state managed in global `cart` array
- Persistent storage via `localStorage` (key: 'cart')

**Key Functions:**
- `renderMenu(category)` - Renders menu items for a specific category
- `addToCart(itemId)` - Adds item to cart or increments quantity
- `updateCart()` - Re-renders cart display and updates totals
- `updateQuantity(itemId, change)` - Modifies item quantity (+/-)
- `checkout()` - Generates order number, shows confirmation modal, clears cart

**State Flow:**
1. User clicks "Ajouter" → `addToCart()` called
2. Cart array updated (new item or quantity++)
3. `updateCart()` re-renders cart sidebar and saves to localStorage
4. Toast notification displayed via `showToast()`

### Naming Conventions
- CSS classes use kebab-case: `.menu-card`, `.cart-sidebar`, `.checkout-btn`
- JavaScript functions use camelCase: `addToCart`, `showMenuCategory`, `toggleMobileCart`
- French language used for all user-facing text (UI, content, alt text)

## Development Workflow

Since this is a static site running on WAMP, there are no build commands. Development workflow:

1. **Edit files** directly in this directory
2. **View in browser** at `http://localhost/restomaquette/` (assuming WAMP is running)
3. **No compilation or build step** required - just refresh browser

## Key Behaviors & Features

### Navigation
- Fixed navbar on index.html with scroll effects (adds `.scrolled` class when `window.scrollY > 50`)
- Smooth scrolling for anchor links (handled via JavaScript event listeners)
- "Commander" button links to commande.html

### Menu Display (index.html)
- Tab-based menu switching using `showCategory()` function
- Menu data hardcoded in JavaScript object
- Categories: entrees, plats, desserts, vins

### Ordering System (commande.html)
- Real-time cart updates with quantity controls
- Mobile-responsive: cart becomes a slide-out sidebar on screens ≤968px
- Checkout shows modal with generated order number (format: `CMD-XXXXXXXXX`)
- LocalStorage persistence ensures cart survives page refreshes

### Animations
- Fade-in animations on scroll using Intersection Observer API (`.fade-in.visible`)
- CSS transitions for hover effects (buttons, cards, links)
- Slide-in animation for cart items using `@keyframes slideIn`

## Common Modifications

### Adding Menu Items
Edit the `menuData` object in either file:
```javascript
const menuData = {
  entrees: [
    { id: 1, name: "...", description: "...", price: 28, icon: '<svg>...</svg>' }
  ]
};
```

### Changing Theme Colors
Modify CSS custom properties in `:root`:
```css
:root {
  --primary-color: #000000;  /* Black */
  --accent-color: #dc143c;   /* Crimson red - brand color for "L'Attellu" */
}
```

**Important:** The brand name "L'Attellu" should always be displayed in the accent color (red).

### Adjusting Delivery Times
Update the `<select id="deliveryTime">` options in commande.html

## Notes
- No backend - this is purely a frontend mockup
- Order numbers are randomly generated client-side (not real order processing)
- No actual payment processing or database integration
- Images sourced from Unsplash (restaurant interiors, plated dishes, chef photos)
- Hero background: Restaurant interior from Unsplash
- Gallery uses real food photography from Unsplash collections
