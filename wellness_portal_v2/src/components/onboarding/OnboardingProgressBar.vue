<template>
  <div class="onb-progress">
    <div class="onb-progress__track">
      <div class="onb-progress__fill" :style="{ width: (current / total) * 100 + '%' }" />
    </div>
    <span class="onb-progress__label">{{ t('onboarding.step_of', { current, total }) }}</span>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'
const { t } = useI18n()
defineProps({ current: Number, total: Number })
</script>

<style scoped>
.ob-progress { display: flex; flex-direction: column; align-items: center; gap: 8px; padding: 16px var(--container-pad) 0; }
.ob-progress__track {
  width: 100%; max-width: 480px; height: 3px; background: var(--ink-100);
  border-radius: 1px; overflow: hidden;
}
.ob-progress__fill {
  height: 100%; background: var(--sage-500);
  transition: width var(--duration-slow) var(--ease-smooth); border-radius: 1px;
}
.ob-progress__label { font-size: 11px; font-weight: 600; color: var(--text-muted); text-transform: uppercase; letter-spacing: 2px; }
</style>
