<template>
  <div class="step-select">
    <h2 class="step-title">{{ t('onboarding.step6_title') }}</h2>
    <div class="pace-grid">
      <button
        v-for="opt in options" :key="opt.id"
        class="pace-card" :class="{ 'pace-card--active': selected === opt.id }"
        @click="selected = opt.id"
      >
        <Icon :icon="ONBOARDING_PACE_ICONS[opt.id]" class="pace-card__icon app-icon app-icon--lg app-icon--primary" />
        <strong class="pace-card__label">{{ t(opt.labelKey) }}</strong>
        <span class="pace-card__desc">{{ t(opt.descKey) }}</span>
      </button>
    </div>
    <button class="step-btn" :disabled="!selected" @click="emit('next', { learningPace: selected })">
      {{ t('onboarding.btn_continue') }}
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useI18n } from 'vue-i18n'
import { ONBOARDING_PACE_ICONS } from '@/constants/appIcons'

const { t } = useI18n()
const emit = defineEmits(['next'])
const selected = ref('')
const options = [
  { id: 'fast',     labelKey: 'onboarding.pace_fast',     descKey: 'onboarding.pace_fast_desc'     },
  { id: 'balanced', labelKey: 'onboarding.pace_balanced', descKey: 'onboarding.pace_balanced_desc' },
  { id: 'slow',     labelKey: 'onboarding.pace_slow',     descKey: 'onboarding.pace_slow_desc'     }
]
</script>

<style scoped>
.step { display: flex; flex-direction: column; align-items: center; gap: 24px; padding: 48px var(--container-pad); min-height: 60vh; justify-content: center; max-width: 560px; margin: 0 auto; }
.step__title { font-family: var(--font-display); font-size: clamp(26px,4vw,38px); font-weight: 300; color: var(--ink-900); text-align: center; line-height: 1.2; }
.step__sub   { font-size: 16px; color: var(--text-secondary); text-align: center; line-height: 1.7; }
.step__options { display: flex; flex-direction: column; gap: 10px; width: 100%; }
.step-opt {
  display: flex; align-items: center; gap: 14px;
  padding: 16px 20px; border-radius: var(--radius);
  background: var(--bg-surface); border: 2px solid var(--ink-200);
  cursor: pointer; transition: all var(--duration-fast);
  box-shadow: 2px 2px 0 var(--ink-100);
}
.step-opt:hover { border-color: var(--sage-300); transform: translate(-1px,-1px); box-shadow: 3px 3px 0 var(--sage-100); }
.step-opt--active { border-color: var(--sage-500); background: var(--sage-50); box-shadow: 3px 3px 0 var(--sage-200); }
.step-opt__icon { color: var(--sage-500); flex-shrink: 0; }
.step-opt__label { font-size: 15px; font-weight: 600; color: var(--ink-900); }
.step-opt__sub   { font-size: 13px; color: var(--text-muted); margin-top: 2px; }
.step-opt__check {
  margin-left: auto; width: 22px; height: 22px; border-radius: var(--radius-sm);
  border: 2px solid var(--ink-200); display: flex; align-items: center; justify-content: center; flex-shrink: 0;
  transition: all var(--duration-fast);
}
.step-opt--active .step-opt__check { background: var(--sage-500); border-color: var(--sage-600); color: white; }
.step__cta {
  display: flex; align-items: center; justify-content: center; gap: 8px;
  width: 100%; padding: 15px; border-radius: var(--radius-sm);
  background: var(--ink-900); color: var(--parchment);
  border: 2px solid var(--ink-900); font-size: 15px; font-weight: 700;
  font-family: var(--font-body); cursor: pointer; box-shadow: 4px 4px 0 var(--sage-400);
  transition: all var(--duration-fast);
}
.step__cta:hover { background: var(--sage-600); border-color: var(--sage-600); box-shadow: 4px 4px 0 var(--sage-800); transform: translate(-1px,-1px); }
.step__cta:disabled { opacity: 0.5; pointer-events: none; }
</style>
