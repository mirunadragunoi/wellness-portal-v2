<template>
  <div class="step">
    <h2 class="step__title">{{ t('onboarding.step1_title') }}</h2>
    <p class="step__sub">{{ t('onboarding.step1_subtitle') }}</p>
    <div class="objectives__grid">
      <button
        v-for="obj in objectives" :key="obj.id"
        class="obj-card"
        :class="{ 'obj-card--active': selected.includes(obj.id) }"
        @click="toggle(obj.id)"
      >
        <Icon :icon="ONBOARDING_OBJECTIVE_ICONS[obj.id]" class="obj-card__icon app-icon app-icon--lg app-icon--primary" />
        <span class="obj-card__label">{{ t(obj.labelKey) }}</span>
      </button>
    </div>
    <button class="step__btn" :disabled="!selected.length" @click="emit('next', { objectives: selected })">
      {{ t('onboarding.btn_continue') }}
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useI18n } from 'vue-i18n'
import { ONBOARDING_OBJECTIVE_ICONS } from '@/constants/appIcons'

const { t } = useI18n()
const emit = defineEmits(['next'])
const selected = ref([])

const objectives = [
  { id: 'stress',   labelKey: 'onboarding.obj_stress'   },
  { id: 'sleep',    labelKey: 'onboarding.obj_sleep'    },
  { id: 'focus',    labelKey: 'onboarding.obj_focus'    },
  { id: 'mood',     labelKey: 'onboarding.obj_mood'     },
  { id: 'meditate', labelKey: 'onboarding.obj_meditate' },
  { id: 'growth',   labelKey: 'onboarding.obj_growth'   }
]

function toggle(id) {
  const idx = selected.value.indexOf(id)
  if (idx >= 0) selected.value.splice(idx, 1)
  else selected.value.push(id)
}
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
