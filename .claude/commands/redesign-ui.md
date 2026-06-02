---
description: Redesign the Vue 3 app from a top navigation bar layout into a modern SaaS-style left sidebar layout
---

# Redesign UI: Top Nav → Left Sidebar SaaS Layout

You are redesigning the Factory Inventory Management app from a horizontal top-navigation bar layout into a modern SaaS-style application with a fixed left sidebar.

**MANDATORY RULE**: ANY creation or significant modification of a `.vue` file MUST be delegated to the `vue-expert` agent via the Agent tool. Do not write `.vue` file content yourself. Compose clear, precise instructions for vue-expert for each delegation.

---

## Step 0 — Read Current State

Before delegating anything, read these two files yourself to confirm their exact current structure:

1. Read `client/src/App.vue` — note the full `<style>` block (global CSS classes like `.card`, `.badge`, `.stat-card`, table rules must be preserved), the template structure, and which events `ProfileMenu` emits.
2. Read `client/src/components/FilterBar.vue` — note the `position: sticky; top: 70px; z-index: 90` rules that must be removed.

Do not proceed until you have read both files.

---

## Step 1 — Create `AppSidebar.vue`

Delegate to `vue-expert` with this exact task:

> Create `client/src/components/AppSidebar.vue` — a fixed left sidebar component for the SaaS layout redesign.
>
> **Template structure (top to bottom):**
> - A root `<aside class="app-sidebar">` element
> - Logo section (`<div class="sidebar-logo">`): display `t('nav.companyName')` in white bold text and `t('nav.subtitle')` below it in slate-400 color (`#94a3b8`)
> - Navigation section (`<nav class="sidebar-nav">`): six `<router-link>` entries, each with class `nav-item` that conditionally adds `active` class based on exact route match. Each nav item contains an inline SVG icon followed by a text label from i18n.
> - Bottom section (`<div class="sidebar-footer">`): `LanguageSwitcher` above `ProfileMenu`, stacked vertically
>
> **Nav items, i18n keys, routes, and SVG icon paths** (all icons: `viewBox="0 0 24 24"`, `fill="none"`, `stroke="currentColor"`, `stroke-width="1.5"`, rendered at `18×18`):
>
> 1. **Dashboard** → route `/`, key `t('nav.overview')`
>    - SVG: four rounded rectangles in a 2×2 grid
>    - `<rect x="3" y="3" width="7" height="7" rx="1"/>` `<rect x="14" y="3" width="7" height="7" rx="1"/>` `<rect x="3" y="14" width="7" height="7" rx="1"/>` `<rect x="14" y="14" width="7" height="7" rx="1"/>`
>
> 2. **Inventory** → route `/inventory`, key `t('nav.inventory')`
>    - SVG: package/box
>    - `<path d="M21 16V8a2 2 0 00-1-1.73l-7-4a2 2 0 00-2 0l-7 4A2 2 0 003 8v8a2 2 0 001 1.73l7 4a2 2 0 002 0l7-4A2 2 0 0021 16z"/>` `<polyline points="3.27 6.96 12 12.01 20.73 6.96"/>` `<line x1="12" y1="22.08" x2="12" y2="12"/>`
>
> 3. **Orders** → route `/orders`, key `t('nav.orders')`
>    - SVG: clipboard with lines
>    - `<path d="M16 4h2a2 2 0 012 2v14a2 2 0 01-2 2H6a2 2 0 01-2-2V6a2 2 0 012-2h2"/>` `<rect x="8" y="2" width="8" height="4" rx="1" ry="1"/>` `<line x1="9" y1="12" x2="15" y2="12"/>` `<line x1="9" y1="16" x2="13" y2="16"/>`
>
> 4. **Demand** → route `/demand`, key `t('nav.demandForecast')`
>    - SVG: trending-up arrow
>    - `<polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/>` `<polyline points="16 7 22 7 22 13"/>`
>
> 5. **Finance** → route `/spending`, key `t('nav.finance')`
>    - SVG: dollar coin
>    - `<circle cx="12" cy="12" r="9"/>` `<path d="M14.83 9.17a3 3 0 00-5.66 1.36c0 1.66 1.34 2.5 3 3 1.66.5 3 1.34 3 3a3 3 0 01-5.66 1.36"/>` `<line x1="12" y1="6" x2="12" y2="8"/>` `<line x1="12" y1="16" x2="12" y2="18"/>`
>
> 6. **Reports** → route `/reports`, label `'Reports'` (no i18n key exists yet)
>    - SVG: bar chart (three vertical bars)
>    - `<line x1="18" y1="20" x2="18" y2="10"/>` `<line x1="12" y1="20" x2="12" y2="4"/>` `<line x1="6" y1="20" x2="6" y2="14"/>`
>
> **Active state logic** — use `useRoute()` from `vue-router`. Bind `:class="{ active: route.path === '/' }"` for Dashboard, `:class="{ active: route.path === '/inventory' }"` for Inventory, and so on for each route. Use exact equality — NOT `startsWith` (which would wrongly make `/` match everything).
>
> **Emits:** The component must declare `emits: ['show-profile-details', 'show-tasks']` and forward those events from `ProfileMenu` via `@show-profile-details="$emit('show-profile-details')"` and `@show-tasks="$emit('show-tasks')"`.
>
> **Script imports needed:**
> - `useRoute` from `vue-router`
> - `useI18n` from `../composables/useI18n`
> - `LanguageSwitcher` from `./LanguageSwitcher.vue`
> - `ProfileMenu` from `./ProfileMenu.vue`
>
> **Scoped CSS** (add as `<style scoped>`):
>
> ```css
> .app-sidebar {
>   width: 240px;
>   min-width: 240px;
>   background: #0f172a;
>   height: 100vh;
>   display: flex;
>   flex-direction: column;
>   position: sticky;
>   top: 0;
>   overflow-y: auto;
>   flex-shrink: 0;
>   z-index: 100;
> }
>
> .sidebar-logo {
>   padding: 1.5rem 1.25rem 1.25rem;
>   border-bottom: 1px solid rgba(255, 255, 255, 0.08);
>   flex-shrink: 0;
> }
>
> .sidebar-logo .company-name {
>   font-size: 1.125rem;
>   font-weight: 700;
>   color: #ffffff;
>   letter-spacing: -0.025em;
>   line-height: 1.3;
> }
>
> .sidebar-logo .company-subtitle {
>   font-size: 0.75rem;
>   color: #94a3b8;
>   margin-top: 0.25rem;
>   font-weight: 400;
> }
>
> .sidebar-nav {
>   flex: 1;
>   padding: 0.75rem;
>   display: flex;
>   flex-direction: column;
>   gap: 0.125rem;
> }
>
> .nav-item {
>   display: flex;
>   align-items: center;
>   gap: 0.75rem;
>   padding: 0.625rem 0.875rem;
>   border-radius: 6px;
>   text-decoration: none;
>   font-size: 0.875rem;
>   font-weight: 500;
>   color: #94a3b8;
>   transition: all 0.15s ease;
>   border-left: 3px solid transparent;
> }
>
> .nav-item:hover {
>   background: rgba(255, 255, 255, 0.06);
>   color: #ffffff;
> }
>
> .nav-item.active {
>   background: rgba(37, 99, 235, 0.25);
>   border-left-color: #2563eb;
>   color: #ffffff;
> }
>
> .nav-item svg {
>   flex-shrink: 0;
>   width: 18px;
>   height: 18px;
> }
>
> .sidebar-footer {
>   padding: 0.875rem 0.75rem;
>   border-top: 1px solid rgba(255, 255, 255, 0.08);
>   display: flex;
>   flex-direction: column;
>   gap: 0.5rem;
>   flex-shrink: 0;
> }
> ```

---

## Step 2 — Update `FilterBar.vue`

Delegate to `vue-expert` with this exact task:

> Modify `client/src/components/FilterBar.vue` to remove its sticky positioning so it renders inline as a strip above the main content area.
>
> **Specific CSS-only changes to the `.filters-bar` rule:**
> - Remove `position: sticky`
> - Remove `top: 70px`
> - Remove `z-index: 90`
> - Change `background` to `#ffffff`
> - Add `border-bottom: 1px solid #e2e8f0`
>
> Do not change the template or script. Keep all other CSS rules (`.filters-container`, `.filter-group`, `.filter-label`, `.filter-select`, `.reset-filters-btn`, etc.) exactly as they are.

---

## Step 3 — Update `App.vue`

Wait until Step 1 is complete (AppSidebar.vue exists), then delegate to `vue-expert` with this exact task:

> Modify `client/src/App.vue` to implement the SaaS sidebar layout. This is a structural refactor — preserve all existing global CSS utility rules carefully.
>
> **Template changes:**
>
> 1. Remove the entire `<header class="top-nav">...</header>` block (logo, nav-tabs, LanguageSwitcher, ProfileMenu).
> 2. Add `<AppSidebar @show-profile-details="showProfileDetails = true" @show-tasks="showTasks = true" />` as the first child of `<div class="app">`.
> 3. Wrap `<FilterBar />` and `<main class="main-content">` together in a new `<div class="app-body">` div.
>
> The resulting template skeleton must be:
> ```
> <div class="app">
>   <AppSidebar @show-profile-details="showProfileDetails = true" @show-tasks="showTasks = true" />
>   <div class="app-body">
>     <FilterBar />
>     <main class="main-content">
>       <router-view />
>     </main>
>   </div>
>   <ProfileDetailsModal :is-open="showProfileDetails" @close="showProfileDetails = false" />
>   <TasksModal :is-open="showTasks" :tasks="tasks" @close="showTasks = false" @add-task="addTask" @delete-task="deleteTask" @toggle-task="toggleTask" />
> </div>
> ```
>
> **Script changes:**
> - Add `import AppSidebar from './components/AppSidebar.vue'` to imports
> - Add `AppSidebar` to the `components: {}` object
> - Remove `import LanguageSwitcher from './components/LanguageSwitcher.vue'` and remove it from `components: {}`
> - Remove `import ProfileMenu from './components/ProfileMenu.vue'` and remove it from `components: {}`
> - Check if `useI18n` (`t`) is still used in App.vue's own template — if not, remove that import and composable call from `setup()`
> - Check if `useAuth` is still used in App.vue's own `setup()` — remove only if unused
>
> **CSS changes in the unscoped `<style>` block:**
>
> 1. Change `.app` rule to use `flex-direction: row`:
>    ```css
>    .app {
>      display: flex;
>      flex-direction: row;
>      min-height: 100vh;
>    }
>    ```
>
> 2. Add new `.app-body` rule immediately after `.app`:
>    ```css
>    .app-body {
>      flex: 1;
>      display: flex;
>      flex-direction: column;
>      min-width: 0;
>      overflow-x: hidden;
>    }
>    ```
>
> 3. Update `.main-content` — remove `max-width` and `margin: 0 auto`:
>    ```css
>    .main-content {
>      flex: 1;
>      padding: 1.5rem 2rem;
>    }
>    ```
>
> 4. Remove these rule blocks (old top-nav styles only):
>    - `.top-nav { ... }`
>    - `.nav-container { ... }`
>    - `.nav-container > .nav-tabs { ... }`
>    - `.nav-container > .language-switcher { ... }`
>    - `.logo { ... }`
>    - `.logo h1 { ... }`
>    - `.subtitle { ... }`
>    - `.nav-tabs { ... }`
>    - `.nav-tabs a { ... }`
>    - `.nav-tabs a:hover { ... }`
>    - `.nav-tabs a.active { ... }`
>    - `.nav-tabs a.active::after { ... }`
>
> 5. **CRITICAL: Do not remove or modify any of these rules** — `.page-header`, `.page-header h2`, `.page-header p`, `.stats-grid`, `.stat-card`, `.stat-card:hover`, `.stat-label`, `.stat-value`, `.stat-card.warning`, `.stat-card.success`, `.stat-card.danger`, `.stat-card.info`, `.card`, `.card-header`, `.card-title`, `.table-container`, `table`, `thead`, `th`, `td`, `tbody tr`, `tbody tr:hover`, `.badge` (and all `.badge.*` variants), `.loading`, `.error`
>
> 6. Add these global overrides at the end of the `<style>` block so LanguageSwitcher and ProfileMenu buttons are readable on the dark sidebar:
>    ```css
>    /* Sidebar footer: adapt light-theme widgets for dark background */
>    .app-sidebar .language-button,
>    .app-sidebar .profile-button {
>      background: rgba(255, 255, 255, 0.08);
>      border-color: rgba(255, 255, 255, 0.12);
>      color: #e2e8f0;
>    }
>
>    .app-sidebar .language-button:hover,
>    .app-sidebar .profile-button:hover {
>      background: rgba(255, 255, 255, 0.14);
>      border-color: rgba(255, 255, 255, 0.2);
>    }
>    ```

---

## Step 4 — Verify

After all three vue-expert delegations complete:

1. Confirm `client/src/components/AppSidebar.vue` exists and contains `<aside class="app-sidebar">`, all six nav links with SVG icons, and the sidebar-footer with LanguageSwitcher and ProfileMenu.
2. Confirm `client/src/App.vue` no longer contains `<header class="top-nav">`, now has `<AppSidebar>` and `<div class="app-body">`, `.app` uses `flex-direction: row`, and all global utility classes (`.card`, `.badge`, `.stat-card`, etc.) are still present.
3. Confirm `client/src/components/FilterBar.vue` no longer has `position: sticky` or `top: 70px`.
4. If the frontend server is running at `http://localhost:3000`, use Playwright MCP to take a snapshot and visually confirm the sidebar is on the left, nav links are visible, and the main content renders correctly.

---

## Target Layout Reference

```
┌──────────────┬────────────────────────────────────┐
│  AppSidebar  │  FilterBar (white strip)            │
│  240px       ├────────────────────────────────────┤
│  #0f172a     │  <router-view> content              │
│  sticky      │  padding: 1.5rem 2rem               │
│  full-h      │                                     │
└──────────────┴────────────────────────────────────┘
```

Active nav: `background: rgba(37,99,235,0.25)` + `border-left: 3px solid #2563eb` + white text.
Hover nav: `background: rgba(255,255,255,0.06)` + white text.
