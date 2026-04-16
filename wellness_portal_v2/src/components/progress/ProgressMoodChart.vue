<template>
  <div class="mood-chart-card">
    <div class="mood-chart-card__header">
      <h3 class="mood-chart-card__title">{{ t('progress.mood_history') }}</h3>
      <span class="mood-chart-card__sub">{{ t('progress.last_7_days') }}</span>
    </div>

    <div class="mood-chart-card__chart">
      <div class="mood-bars">
        <div v-for="day in moodStore.last7Days" :key="day.date" class="mood-bar-wrap">
          <div class="mood-bar-col">
            <Icon
              v-if="day.icon"
              :icon="day.icon"
              class="mood-bar-icon app-icon app-icon--sm app-icon--muted"
            />
            <div
              class="mood-bar"
              :style="{
                height: day.value ? (day.value / 5 * 100) + '%' : '4px',
                background: day.value ? moodGradient(day.value) : 'var(--bg-muted)'
              }"
            />
          </div>
          <span class="mood-bar-label">{{ day.label }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'
import { useMoodStore } from '@/stores/mood'

const { t }     = useI18n()
const moodStore = useMoodStore()

function moodGradient(value) {
  const colors = {
    1: '#94a3b8',
    2: '#64748b',
    3: 'var(--sage-300)',
    4: 'var(--sage-500)',
    5: '#f59e0b'
  }
  return colors[value] || '#e2e8f0'
}
</script>

<style scoped>
.mood-chart {
  background: var(--bg-surface); border: 2px solid var(--ink-200);
  border-radius: var(--radius-lg); padding: 20px; box-shadow: 3px 3px 0 var(--ink-100);
}
.mood-chart__title {
  font-family: var(--font-display); font-size: 18px; font-weight: 400;
  color: var(--ink-900); margin-bottom: 20px;
  padding-bottom: 12px; border-bottom: 1.5px solid var(--ink-100);
}
.mood-chart__canvas { width: 100%; height: 180px; position: relative; }
.mood-chart__bars { display: flex; align-items: flex-end; gap: 6px; height: 140px; padding: 0 4px; }
.mood-bar-col { display: flex; flex-direction: column; align-items: center; gap: 6px; flex: 1; }
.mood-bar {
  width: 100%; border-radius: var(--radius-sm) var(--radius-sm) 0 0;
  transition: height var(--duration-slow) var(--ease-smooth); min-height: 4px;
}
.mood-bar-label { font-size: 10px; color: var(--text-muted); font-weight: 600; text-transform: uppercase; }
.mood-chart__legend { display: flex; gap: 14px; flex-wrap: wrap; margin-top: 14px; padding-top: 12px; border-top: 1px solid var(--ink-100); }
.legend-item { display: flex; align-items: center; gap: 6px; font-size: 12px; color: var(--text-secondary); }
.legend-dot { width: 10px; height: 10px; border-radius: var(--radius-sm); flex-shrink: 0; }
.mood-chart__empty { padding: 40px 0; text-align: center; color: var(--text-muted); font-size: 14px; }
</style>
