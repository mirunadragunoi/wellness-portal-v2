<template>
  <div class="step-proof">
    <div class="proof-visual">
      <div class="proof-orb">
        <span class="proof-orb__num">95%</span>
        <span class="proof-orb__label">less stress</span>
      </div>
    </div>
    <h2 class="proof-title">{{ t('onboarding.step2_title') }}</h2>
    <p class="proof-body">{{ t('onboarding.step2_body') }}</p>
    <button class="step-btn" @click="emit('next')">{{ t('onboarding.btn_continue') }}</button>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'
const { t } = useI18n()
const emit = defineEmits(['next'])
</script>

<style scoped>
.social-proof { display: flex; flex-direction: column; align-items: center; gap: 24px; padding: 48px var(--container-pad); min-height: 70vh; justify-content: center; max-width: 560px; margin: 0 auto; text-align: center; }
.social-proof__media { width: 200px; height: 200px; border-radius: var(--radius-lg); background-size: cover; background-position: center; border: 3px solid var(--ink-200); box-shadow: 6px 6px 0 var(--ink-200); }
.social-proof__stat { font-family: var(--font-display); font-size: 56px; font-weight: 300; color: var(--ink-900); letter-spacing: -2px; }
.social-proof__label { font-family: var(--font-display); font-size: 22px; font-weight: 300; color: var(--ink-900); }
.social-proof__sub { font-size: 16px; color: var(--text-secondary); line-height: 1.7; max-width: 400px; }
.social-proof__cta {
  display: flex; align-items: center; justify-content: center; gap: 8px;
  width: 100%; max-width: 340px; padding: 15px;
  border-radius: var(--radius-sm); background: var(--ink-900); color: var(--parchment);
  border: 2px solid var(--ink-900); font-size: 15px; font-weight: 700;
  font-family: var(--font-body); cursor: pointer; box-shadow: 4px 4px 0 var(--sage-400);
  transition: all var(--duration-fast);
}
.social-proof__cta:hover { background: var(--sage-600); border-color: var(--sage-600); transform: translate(-1px,-1px); }
</style>
