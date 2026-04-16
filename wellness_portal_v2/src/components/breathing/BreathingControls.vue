<template>
  <div class="controls">
    <div class="controls__duration">
      <span class="controls__duration-label">{{ t('breathing.duration_label') }}</span>
      <div class="controls__duration-pills">
        <button
          v-for="sec in durations" :key="sec"
          class="dur-pill"
          :class="{ 'dur-pill--active': modelValue === sec }"
          :disabled="isRunning"
          @click="emit('update:modelValue', sec)"
        >
          {{ sec / 60 }} min
        </button>
      </div>
    </div>

    <div class="controls__btns">
      <template v-if="!isRunning">
        <button class="ctrl-btn ctrl-btn--start" @click="emit('start')">
          {{ t('breathing.start') }}
        </button>
      </template>
      <template v-else>
        <button class="ctrl-btn ctrl-btn--pause" @click="emit('pause')">
          {{ isPaused ? t('breathing.start') : t('breathing.pause_btn') }}
        </button>
        <button class="ctrl-btn ctrl-btn--stop" @click="emit('stop')">
          {{ t('breathing.stop') }}
        </button>
      </template>
    </div>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'
const { t } = useI18n()
defineProps({
  modelValue: Number,
  durations:  Array,
  isRunning:  Boolean,
  isPaused:   Boolean
})
const emit = defineEmits(['update:modelValue', 'start', 'pause', 'stop'])
</script>

<style scoped>
.b-controls { display: flex; flex-direction: column; align-items: center; gap: 16px; width: 100%; max-width: 400px; }
.b-controls__patterns { display: flex; gap: 8px; flex-wrap: wrap; justify-content: center; }
.pattern-btn {
  padding: 8px 16px; border-radius: var(--radius-sm);
  font-size: 13px; font-weight: 600; cursor: pointer;
  background: rgba(255,255,255,0.06); color: rgba(255,255,255,0.6);
  border: 1.5px solid rgba(255,255,255,0.12);
  transition: all var(--duration-fast); font-family: var(--font-body);
}
.pattern-btn:hover { border-color: var(--sage-400); color: var(--sage-300); background: rgba(71,127,60,0.1); }
.pattern-btn--active { background: var(--sage-600); color: white; border-color: var(--sage-500); box-shadow: 2px 2px 0 var(--sage-800); }

.b-controls__action { display: flex; gap: 12px; align-items: center; }
.btn-main {
  display: flex; align-items: center; justify-content: center; gap: 8px;
  padding: 13px 36px; border-radius: var(--radius-sm);
  background: var(--sage-500); color: white; border: 2px solid var(--sage-600);
  font-size: 15px; font-weight: 700; cursor: pointer; font-family: var(--font-body);
  box-shadow: 3px 3px 0 var(--sage-700); transition: all var(--duration-fast);
}
.btn-main:hover { background: var(--sage-400); transform: translate(-1px,-1px); box-shadow: 4px 4px 0 var(--sage-700); }
.btn-stop {
  padding: 13px 20px; border-radius: var(--radius-sm);
  background: transparent; color: var(--ink-400);
  border: 2px solid var(--ink-700); font-size: 14px; font-weight: 600;
  cursor: pointer; font-family: var(--font-body); transition: all var(--duration-fast);
}
.btn-stop:hover { color: var(--parchment); border-color: var(--ink-400); }
</style>
