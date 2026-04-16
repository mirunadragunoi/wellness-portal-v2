<template>
  <button class="sos-button" @click="uiStore.openSOS()" :aria-label="t('nav.sos')">
    <span class="sos-button__ring" />
    <span class="sos-button__inner">SOS</span>
  </button>
</template>

<script setup>
import { useI18n } from 'vue-i18n'
import { useUIStore } from '@/stores/ui'
const { t } = useI18n()
const uiStore = useUIStore()
</script>

<style scoped>
.sos-fab {
  position: fixed; bottom: 28px; right: 20px; z-index: 60;
  width: 52px; height: 52px; border-radius: var(--radius);
  background: #dc2626; color: white; font-size: 11px; font-weight: 800;
  letter-spacing: 0.5px; border: 3px solid #b91c1c;
  box-shadow: 4px 4px 0 rgba(0,0,0,0.3);
  display: flex; align-items: center; justify-content: center;
  cursor: pointer; transition: all var(--duration-fast);
}
.sos-fab:hover { transform: translate(-2px,-2px); box-shadow: 6px 6px 0 rgba(0,0,0,0.3); }
.sos-fab:active { transform: translate(0,0); box-shadow: 2px 2px 0 rgba(0,0,0,0.3); }
@media (max-width: 768px) { .sos-fab { display: none; } }
</style>
