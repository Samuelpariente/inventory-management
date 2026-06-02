---
name: saas-redesign
description: Design system and component patterns for the SaaS sidebar layout of the Factory Inventory Management app. Load when working on AppSidebar, the App.vue layout, or any sidebar-related UI.
---

# SaaS Redesign — Design System Reference

This skill defines the design tokens, layout patterns, and component guidelines for the left-sidebar SaaS layout of the inventory management app.

---

## Design Tokens

### Colors

| Token | Value | Usage |
|---|---|---|
| Sidebar bg | `#0f172a` | Sidebar background (slate-900) |
| Sidebar divider | `rgba(255,255,255,0.08)` | Logo/footer border lines |
| Active bg | `rgba(37,99,235,0.25)` | Active nav item background |
| Active border | `#2563eb` | Active nav item left border |
| Hover bg | `rgba(255,255,255,0.06)` | Nav item hover background |
| Nav text muted | `#94a3b8` | Inactive nav labels, subtitle |
| Nav text active | `#ffffff` | Active/hovered nav labels |
| Footer btn bg | `rgba(255,255,255,0.08)` | LanguageSwitcher/ProfileMenu button bg |
| Footer btn border | `rgba(255,255,255,0.12)` | Footer button border |
| Content bg | `#f8fafc` | Main content area |
| Filter bar bg | `#ffffff` | FilterBar background |
| Primary | `#2563eb` | Blue accent |
| Border default | `#e2e8f0` | Standard borders |

### Spacing

| Name | Value | Usage |
|---|---|---|
| Nav gap | `0.125rem` | Gap between nav items |
| Nav icon gap | `0.75rem` | Gap between icon and label in nav item |
| Nav padding | `0.625rem 0.875rem` | Nav item internal padding |
| Sidebar padding | `0.75rem` | Nav section padding |
| Logo padding | `1.5rem 1.25rem 1.25rem` | Sidebar logo section |
| Footer padding | `0.875rem 0.75rem` | Sidebar footer section |
| Content padding | `1.5rem 2rem` | Main content area |

---

## Layout Pattern

### App Shell

```
.app (flex-direction: row; min-height: 100vh)
├── AppSidebar (width: 240px; flex-shrink: 0; sticky; height: 100vh)
└── .app-body (flex: 1; flex-direction: column; min-width: 0)
    ├── FilterBar (no sticky; white bg; border-bottom)
    └── .main-content (flex: 1; padding: 1.5rem 2rem)
        └── <router-view>
```

**Key rules:**
- `.app` must be `flex-direction: row` (not column)
- `AppSidebar` must have `flex-shrink: 0` so it never compresses
- `.app-body` must have `min-width: 0` so flex children can shrink correctly
- `FilterBar` must NOT have `position: sticky` or `top:` offset (remove both)

### Sidebar Internal Structure

```
<aside class="app-sidebar"> (flex column; height: 100vh; overflow-y: auto)
├── .sidebar-logo (flex-shrink: 0; border-bottom)
├── .sidebar-nav (flex: 1; flex column; gap: 0.125rem)
│   └── router-link.nav-item × 6
└── .sidebar-footer (flex-shrink: 0; border-top; flex column; gap: 0.5rem)
    ├── LanguageSwitcher
    └── ProfileMenu
```

---

## Component: AppSidebar

**File:** `client/src/components/AppSidebar.vue`

**Script requirements:**
- Uses `useRoute()` from `vue-router` for active state
- Uses `useI18n()` from `../composables/useI18n` for nav labels
- Declares `emits: ['show-profile-details', 'show-tasks']`
- Forwards those events from ProfileMenu upward

**Active state logic — use exact equality, never `startsWith`:**
```javascript
// Dashboard: exact match (startsWith('/') would match every route)
route.path === '/'

// All others: exact segment match
route.path === '/inventory'
route.path === '/orders'
route.path === '/demand'
route.path === '/spending'
route.path === '/reports'
```

**Nav item template pattern:**
```html
<router-link to="/inventory" class="nav-item" :class="{ active: route.path === '/inventory' }">
  <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
    <!-- paths here -->
  </svg>
  {{ t('nav.inventory') }}
</router-link>
```

---

## SVG Icon Reference

All icons: `width="18" height="18"`, `viewBox="0 0 24 24"`, `fill="none"`, `stroke="currentColor"`, `stroke-width="1.5"`.

| Route | Icon | Paths |
|---|---|---|
| `/` | Grid/Dashboard | `<rect x="3" y="3" width="7" height="7" rx="1"/>` `<rect x="14" y="3" width="7" height="7" rx="1"/>` `<rect x="3" y="14" width="7" height="7" rx="1"/>` `<rect x="14" y="14" width="7" height="7" rx="1"/>` |
| `/inventory` | Package/Box | `<path d="M21 16V8a2 2 0 00-1-1.73l-7-4a2 2 0 00-2 0l-7 4A2 2 0 003 8v8a2 2 0 001 1.73l7 4a2 2 0 002 0l7-4A2 2 0 0021 16z"/>` `<polyline points="3.27 6.96 12 12.01 20.73 6.96"/>` `<line x1="12" y1="22.08" x2="12" y2="12"/>` |
| `/orders` | Clipboard | `<path d="M16 4h2a2 2 0 012 2v14a2 2 0 01-2 2H6a2 2 0 01-2-2V6a2 2 0 012-2h2"/>` `<rect x="8" y="2" width="8" height="4" rx="1"/>` `<line x1="9" y1="12" x2="15" y2="12"/>` `<line x1="9" y1="16" x2="13" y2="16"/>` |
| `/demand` | Trending Up | `<polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/>` `<polyline points="16 7 22 7 22 13"/>` |
| `/spending` | Dollar Coin | `<circle cx="12" cy="12" r="9"/>` `<path d="M14.83 9.17a3 3 0 00-5.66 1.36c0 1.66 1.34 2.5 3 3 1.66.5 3 1.34 3 3a3 3 0 01-5.66 1.36"/>` `<line x1="12" y1="6" x2="12" y2="8"/>` `<line x1="12" y1="16" x2="12" y2="18"/>` |
| `/reports` | Bar Chart | `<line x1="18" y1="20" x2="18" y2="10"/>` `<line x1="12" y1="20" x2="12" y2="4"/>` `<line x1="6" y1="20" x2="6" y2="14"/>` |

---

## Event Flow

```
AppSidebar
  └── ProfileMenu (emits: show-profile-details, show-tasks)
        ↑ forwarded via AppSidebar $emit
App.vue
  ├── @show-profile-details → showProfileDetails = true → ProfileDetailsModal
  └── @show-tasks → showTasks = true → TasksModal
```

ProfileMenu and LanguageSwitcher are rendered **only inside AppSidebar**. They must be removed from App.vue's template and imports.

---

## CSS Rules

### Do

- Use `position: sticky; top: 0; height: 100vh` on the sidebar — not `position: fixed` (keeps document flow intact)
- Use `rgba()` for transparency over the dark sidebar background (not `opacity` on the whole element)
- Override LanguageSwitcher/ProfileMenu button styles via global selectors in `App.vue` (`.app-sidebar .language-button`, etc.) — avoids modifying those components

### Do Not

- Do not use `:deep()` in AppSidebar scoped styles — use global selectors in `App.vue` instead
- Do not add `max-width` or `margin: 0 auto` to `.main-content` (sidebar already constrains the left edge)
- Do not restore `position: sticky` or `top:` offset to FilterBar after this redesign
- Do not remove any global utility classes from `App.vue`'s unscoped `<style>` block (`.card`, `.badge`, `.stat-card`, `table`, `td`, `th`, etc.)
- Do not use emojis in the UI (project-wide rule)

---

## Files Changed by the `/redesign-ui` Command

| File | Change |
|---|---|
| `client/src/components/AppSidebar.vue` | **Created** — new sidebar component |
| `client/src/App.vue` | **Modified** — row layout, remove header, add AppSidebar + .app-body |
| `client/src/components/FilterBar.vue` | **Modified** — remove sticky positioning |

**Not changed:** `main.js`, `ProfileMenu.vue`, `LanguageSwitcher.vue`, all views, all composables.
