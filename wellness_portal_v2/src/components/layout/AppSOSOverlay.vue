<template>
  <Teleport to="body">
    <Transition name="sos-overlay">
      <div v-if="uiStore.sosOpen" class="sos-overlay">
        <div class="sos-panel">
          <!-- Header -->
          <div class="sos-header">
            <div class="sos-badge">
              <Icon icon="lucide:life-buoy" class="app-icon app-icon--3xl app-icon--primary" aria-hidden="true" />
            </div>
            <h2 class="sos-title">{{ t('sos.title') }}</h2>
            <p class="sos-subtitle">{{ t('sos.subtitle') }}</p>
            <button type="button" class="sos-close" :aria-label="t('common.close')" @click="close">
              <Icon icon="lucide:x" class="app-icon app-icon--sm" />
            </button>
          </div>

          <!-- Options -->
          <div v-if="activeMode === null" class="sos-options">
            <button type="button" class="sos-option" @click="activeMode = 'breathing'">
              <span class="sos-option__icon">
                <Icon icon="lucide:wind" class="app-icon app-icon--lg app-icon--primary" aria-hidden="true" />
              </span>
              <div class="sos-option__text">
                <strong>{{ t('sos.option1_title') }}</strong>
                <span>{{ t('sos.option1_desc') }}</span>
              </div>
              <Icon icon="lucide:chevron-right" class="sos-option__arrow app-icon app-icon--md app-icon--muted" aria-hidden="true" />
            </button>
            <button type="button" class="sos-option" @click="activeMode = 'grounding'">
              <span class="sos-option__icon">
                <Icon icon="lucide:sprout" class="app-icon app-icon--lg app-icon--primary" aria-hidden="true" />
              </span>
              <div class="sos-option__text">
                <strong>{{ t('sos.option2_title') }}</strong>
                <span>{{ t('sos.option2_desc') }}</span>
              </div>
              <Icon icon="lucide:chevron-right" class="sos-option__arrow app-icon app-icon--md app-icon--muted" aria-hidden="true" />
            </button>
            <button type="button" class="sos-option" @click="goToSession">
              <span class="sos-option__icon">
                <Icon icon="lucide:headphones" class="app-icon app-icon--lg app-icon--primary" aria-hidden="true" />
              </span>
              <div class="sos-option__text">
                <strong>{{ t('sos.option3_title') }}</strong>
                <span>{{ t('sos.option3_desc') }}</span>
              </div>
              <Icon icon="lucide:chevron-right" class="sos-option__arrow app-icon app-icon--md app-icon--muted" aria-hidden="true" />
            </button>
          </div>

          <!-- Breathing Mode -->
          <div v-else-if="activeMode === 'breathing'" class="sos-breathing">
            <div class="breathing-orb" :class="phaseClass">
              <span class="breathing-orb__phase">{{ currentPhase?.label }}</span>
              <span class="breathing-orb__count">{{ phaseCountdown }}</span>
            </div>
            <p class="breathing-status">{{ isRunning ? formattedRemaining : 'Ready' }}</p>
            <div class="breathing-btns">
              <button v-if="!isRunning" class="sos-btn-primary" @click="start">{{ t('breathing.start') }}</button>
              <template v-else>
                <button class="sos-btn-secondary" @click="pause">{{ isPaused ? t('breathing.start') : t('breathing.pause_btn') }}</button>
                <button class="sos-btn-ghost" @click="stop">{{ t('breathing.stop') }}</button>
              </template>
            </div>
            <div v-if="isComplete" class="sos-check">
              <p>{{ t('sos.feeling_better') }}</p>
              <div class="sos-check__btns">
                <button class="sos-btn-primary" @click="close">{{ t('sos.yes') }}</button>
                <button class="sos-btn-ghost" @click="resetMode">{{ t('sos.no') }}</button>
              </div>
            </div>
          </div>

          <!-- Grounding Mode -->
          <div v-else-if="activeMode === 'grounding'" class="sos-grounding">
            <TransitionGroup name="grounding-step">
              <div class="grounding-step" :key="groundingIndex">
                <div class="grounding-step__number">{{ 5 - groundingIndex }}</div>
                <p class="grounding-step__text">{{ groundingSteps[groundingIndex] }}</p>
              </div>
            </TransitionGroup>

            <div class="grounding-btns">
              <button
                v-if="groundingIndex < groundingSteps.length - 1"
                class="sos-btn-primary"
                @click="groundingIndex++"
              >
                {{ t('sos.grounding_next') }}
              </button>
              <template v-else>
                <p class="sos-done-text">{{ t('sos.grounding_done') }}</p>
                <div class="sos-check__btns">
                  <button class="sos-btn-primary" @click="close">{{ t('sos.yes') }}</button>
                  <button class="sos-btn-ghost"  @click="resetMode">{{ t('sos.no') }}</button>
                </div>
              </template>
            </div>
          </div>

          <!-- Back -->
          <button v-if="activeMode" type="button" class="sos-back" @click="resetMode">
            <Icon icon="lucide:arrow-left" class="app-icon app-icon--xs" aria-hidden="true" />
            {{ t('common.back') }}
          </button>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useI18n } from 'vue-i18n'
import { useRouter } from 'vue-router'
import { useUIStore } from '@/stores/ui'
import { useBreathing } from '@/composables/useBreathing'

const { t } = useI18n()
const uiStore = useUIStore()
const router  = useRouter()

const activeMode    = ref(null)
const groundingIndex = ref(0)

const {
  isRunning, isPaused, isComplete,
  currentPhase, phaseCountdown, formattedRemaining,
  start, pause, stop, reset
} = useBreathing('sos')

const phaseClass = computed(() => ({
  'breathing-orb--inhale': currentPhase.value?.phase === 'inhale',
  'breathing-orb--hold':   currentPhase.value?.phase === 'hold',
  'breathing-orb--exhale': currentPhase.value?.phase === 'exhale'
}))

const groundingSteps = computed(() => [
  t('sos.grounding_5'),
  t('sos.grounding_4'),
  t('sos.grounding_3'),
  t('sos.grounding_2'),
  t('sos.grounding_1')
])

function resetMode() {
  activeMode.value = null
  groundingIndex.value = 0
  reset()
}

function close() {
  resetMode()
  uiStore.closeSOS()
}

function goToSession() {
  close()
  router.push({ name: 'session', params: { id: 'med-010' } })
}
</script>

<style scoped>
.sos-overlay {
  position: fixed; inset: 0; z-index: 300;
  background: rgba(28,26,22,0.92); backdrop-filter: blur(4px);
  display: flex; align-items: center; justify-content: center; padding: 20px;
}
.sos-panel {
  background: var(--parchment); width: 100%; max-width: 480px;
  border: 3px solid var(--ink-900); border-radius: var(--radius-xl);
  overflow: hidden; box-shadow: 10px 10px 0 rgba(0,0,0,0.4);
}
.sos-panel__head {
  background: #dc2626; padding: 18px 24px;
  display: flex; align-items: center; justify-content: space-between;
}
.sos-panel__head-title { font-family: var(--font-display); font-size: 22px; font-weight: 400; color: white; display: flex; align-items: center; gap: 10px; }
.sos-panel__close {
  width: 32px; height: 32px; border-radius: var(--radius-sm);
  background: rgba(255,255,255,0.15); border: none; cursor: pointer;
  color: white; display: flex; align-items: center; justify-content: center; transition: background var(--duration-fast);
}
.sos-panel__close:hover { background: rgba(255,255,255,0.3); }
.sos-panel__body { padding: 24px; display: flex; flex-direction: column; gap: 16px; }
.sos-panel__desc { font-size: 15px; color: var(--text-secondary); line-height: 1.6; }

.sos-tab-group { display: flex; border: 2px solid var(--ink-200); border-radius: var(--radius-sm); overflow: hidden; }
.sos-tab {
  flex: 1; padding: 10px; text-align: center; font-size: 13px; font-weight: 700;
  text-transform: uppercase; letter-spacing: 1px; cursor: pointer;
  background: var(--parchment-2); color: var(--text-secondary); border: none;
  border-right: 2px solid var(--ink-200); font-family: var(--font-body); transition: all var(--duration-fast);
}
.sos-tab:last-child { border-right: none; }
.sos-tab--active { background: var(--ink-900); color: var(--parchment); }

.breathing-orb {
  display: flex; flex-direction: column; align-items: center; gap: 12px; padding: 16px 0;
}
.breathing-orb__circle {
  width: 120px; height: 120px; border-radius: 50%;
  background: linear-gradient(135deg, var(--sage-100), var(--sage-200));
  border: 3px solid var(--sage-300);
  display: flex; align-items: center; justify-content: center;
  animation: breathe-idle 4s ease-in-out infinite;
}
.breathing-orb__phase { font-family: var(--font-display); font-size: 14px; font-style: italic; color: var(--sage-700); font-weight: 400; }
.breathing-orb__count { font-family: var(--font-display); font-size: 40px; font-weight: 300; color: var(--ink-900); }

.ground-list { display: flex; flex-direction: column; gap: 8px; }
.ground-item {
  display: flex; align-items: center; gap: 12px; padding: 12px 14px;
  background: var(--bg-surface); border: 1.5px solid var(--ink-100); border-radius: var(--radius);
  font-size: 14px; color: var(--ink-900);
}
.ground-item__num {
  width: 28px; height: 28px; border-radius: var(--radius-sm);
  background: var(--ink-900); color: var(--parchment);
  display: flex; align-items: center; justify-content: center;
  font-size: 12px; font-weight: 800; flex-shrink: 0;
}

.sos-panel__footer { padding: 0 24px 24px; }
.sos-start-btn {
  display: flex; align-items: center; justify-content: center; gap: 8px;
  width: 100%; padding: 14px; border-radius: var(--radius-sm);
  background: var(--ink-900); color: var(--parchment);
  border: 2px solid var(--ink-900); font-size: 15px; font-weight: 700;
  cursor: pointer; font-family: var(--font-body); box-shadow: 3px 3px 0 var(--sage-400);
  transition: all var(--duration-fast);
}
.sos-start-btn:hover { background: var(--sage-600); border-color: var(--sage-600); transform: translate(-1px,-1px); }

.sos-overlay-enter-active, .sos-overlay-leave-active { transition: all var(--duration-normal) var(--ease-smooth); }
.sos-overlay-enter-from, .sos-overlay-leave-to { opacity: 0; transform: scale(0.96); }
@keyframes breathe-idle { 0%,100%{transform:scale(1)} 50%{transform:scale(1.08)} }
</style>
