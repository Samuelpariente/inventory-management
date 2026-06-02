<script>
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import { useI18n } from '../composables/useI18n'
import LanguageSwitcher from './LanguageSwitcher.vue'
import ProfileMenu from './ProfileMenu.vue'

export default {
  name: 'AppSidebar',
  components: { LanguageSwitcher, ProfileMenu },
  emits: ['show-profile-details', 'show-tasks'],
  setup() {
    const route = useRoute()
    const { t } = useI18n()

    const collapsed = ref(false)

    onMounted(() => {
      if (window.innerWidth < 1024) {
        collapsed.value = true
      }
    })

    return { route, t, collapsed }
  }
}
</script>

<template>
  <aside class="app-sidebar" :class="{ collapsed }">
    <div class="sidebar-logo">
      <div class="logo-content" v-show="!collapsed">
        <div class="company-name">{{ t('nav.companyName') }}</div>
        <div class="company-subtitle">{{ t('nav.subtitle') }}</div>
      </div>
      <button class="sidebar-toggle" @click="collapsed = !collapsed" :title="collapsed ? 'Expand sidebar' : 'Collapse sidebar'">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <template v-if="!collapsed">
            <polyline points="15 18 9 12 15 6"/>
          </template>
          <template v-else>
            <polyline points="9 18 15 12 9 6"/>
          </template>
        </svg>
      </button>
    </div>

    <nav class="sidebar-nav">
      <router-link to="/" class="nav-item" :class="{ active: route.path === '/' }" :title="collapsed ? t('nav.overview') : ''">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <rect x="3" y="3" width="7" height="7" rx="1"/>
          <rect x="14" y="3" width="7" height="7" rx="1"/>
          <rect x="3" y="14" width="7" height="7" rx="1"/>
          <rect x="14" y="14" width="7" height="7" rx="1"/>
        </svg>
        <span class="nav-label">{{ t('nav.overview') }}</span>
      </router-link>

      <router-link to="/inventory" class="nav-item" :class="{ active: route.path === '/inventory' }" :title="collapsed ? t('nav.inventory') : ''">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M21 16V8a2 2 0 00-1-1.73l-7-4a2 2 0 00-2 0l-7 4A2 2 0 003 8v8a2 2 0 001 1.73l7 4a2 2 0 002 0l7-4A2 2 0 0021 16z"/>
          <polyline points="3.27 6.96 12 12.01 20.73 6.96"/>
          <line x1="12" y1="22.08" x2="12" y2="12"/>
        </svg>
        <span class="nav-label">{{ t('nav.inventory') }}</span>
      </router-link>

      <router-link to="/orders" class="nav-item" :class="{ active: route.path === '/orders' }" :title="collapsed ? t('nav.orders') : ''">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M16 4h2a2 2 0 012 2v14a2 2 0 01-2 2H6a2 2 0 01-2-2V6a2 2 0 012-2h2"/>
          <rect x="8" y="2" width="8" height="4" rx="1" ry="1"/>
          <line x1="9" y1="12" x2="15" y2="12"/>
          <line x1="9" y1="16" x2="13" y2="16"/>
        </svg>
        <span class="nav-label">{{ t('nav.orders') }}</span>
      </router-link>

      <router-link to="/demand" class="nav-item" :class="{ active: route.path === '/demand' }" :title="collapsed ? t('nav.demandForecast') : ''">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/>
          <polyline points="16 7 22 7 22 13"/>
        </svg>
        <span class="nav-label">{{ t('nav.demandForecast') }}</span>
      </router-link>

      <router-link to="/spending" class="nav-item" :class="{ active: route.path === '/spending' }" :title="collapsed ? t('nav.finance') : ''">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <circle cx="12" cy="12" r="9"/>
          <path d="M14.83 9.17a3 3 0 00-5.66 1.36c0 1.66 1.34 2.5 3 3 1.66.5 3 1.34 3 3a3 3 0 01-5.66 1.36"/>
          <line x1="12" y1="6" x2="12" y2="8"/>
          <line x1="12" y1="16" x2="12" y2="18"/>
        </svg>
        <span class="nav-label">{{ t('nav.finance') }}</span>
      </router-link>

      <router-link to="/reports" class="nav-item" :class="{ active: route.path === '/reports' }" :title="collapsed ? 'Reports' : ''">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <line x1="18" y1="20" x2="18" y2="10"/>
          <line x1="12" y1="20" x2="12" y2="4"/>
          <line x1="6" y1="20" x2="6" y2="14"/>
        </svg>
        <span class="nav-label">Reports</span>
      </router-link>

      <router-link to="/backlog" class="nav-item" :class="{ active: route.path === '/backlog' }" :title="collapsed ? 'Backlog' : ''">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M10.29 3.86L1.82 18a2 2 0 001.71 3h16.94a2 2 0 001.71-3L13.71 3.86a2 2 0 00-3.42 0z"/>
          <line x1="12" y1="9" x2="12" y2="13"/>
          <line x1="12" y1="17" x2="12.01" y2="17"/>
        </svg>
        <span class="nav-label">Backlog</span>
      </router-link>
    </nav>

    <div class="sidebar-footer">
      <LanguageSwitcher v-show="!collapsed" />
      <ProfileMenu @show-profile-details="$emit('show-profile-details')" @show-tasks="$emit('show-tasks')" />
    </div>
  </aside>
</template>

<style scoped>
.app-sidebar {
  width: 240px;
  min-width: 240px;
  background: #0f172a;
  height: 100vh;
  display: flex;
  flex-direction: column;
  position: sticky;
  top: 0;
  overflow-y: auto;
  flex-shrink: 0;
  z-index: 100;
  transition: width 0.25s ease;
}

.sidebar-logo {
  padding: 1.5rem 1.25rem 1.25rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.logo-content {
  flex: 1;
  min-width: 0;
}

.company-name {
  font-size: 1.125rem;
  font-weight: 700;
  color: #ffffff;
  letter-spacing: -0.025em;
  line-height: 1.3;
}

.company-subtitle {
  font-size: 0.75rem;
  color: #94a3b8;
  margin-top: 0.25rem;
  font-weight: 400;
}

.sidebar-toggle {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  border-radius: 6px;
  border: 1px solid rgba(255, 255, 255, 0.12);
  background: rgba(255, 255, 255, 0.06);
  color: #94a3b8;
  cursor: pointer;
  flex-shrink: 0;
  transition: all 0.15s ease;
}

.sidebar-toggle:hover {
  background: rgba(255, 255, 255, 0.12);
  color: #ffffff;
}

.sidebar-nav {
  flex: 1;
  padding: 0.75rem;
  display: flex;
  flex-direction: column;
  gap: 0.125rem;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 0.875rem;
  border-radius: 6px;
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  color: #94a3b8;
  transition: all 0.15s ease;
  border-left: 3px solid transparent;
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.06);
  color: #ffffff;
}

.nav-item.active {
  background: rgba(37, 99, 235, 0.25);
  border-left-color: #2563eb;
  color: #ffffff;
}

.nav-item svg {
  flex-shrink: 0;
  width: 18px;
  height: 18px;
}

.nav-label {
  white-space: nowrap;
  overflow: hidden;
  transition: opacity 0.15s ease;
}

.sidebar-footer {
  padding: 0.875rem 0.75rem;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  flex-shrink: 0;
}

/* Collapsed state */
.app-sidebar.collapsed {
  width: 64px;
  min-width: 64px;
}

.app-sidebar.collapsed .sidebar-logo {
  justify-content: center;
  padding: 1rem 0.75rem;
}

.app-sidebar.collapsed .nav-label {
  display: none;
}

.app-sidebar.collapsed .nav-item {
  justify-content: center;
  padding: 0.625rem;
  border-left-width: 0;
  border-radius: 8px;
}

.app-sidebar.collapsed .nav-item.active {
  border-left-width: 0;
}

.app-sidebar.collapsed .sidebar-nav {
  padding: 0.75rem 0.5rem;
  align-items: center;
}

.app-sidebar.collapsed .sidebar-footer {
  padding: 0.75rem 0.5rem;
  align-items: center;
}
</style>
