<template>
  <div class="filters">
    <!-- Type -->
    <div class="filter-group">
      <span class="filter-group__label">{{ t('explore.filter_type') }}</span>
      <div class="filter-group__chips">
        <BaseChip small :active="filters.type === 'all'" @click="set('type','all')">{{ t('explore.all') }}</BaseChip>
        <BaseChip v-for="o in typeOptions" :key="o.id" small :active="filters.type === o.id" @click="set('type', o.id)">{{ t(o.key) }}</BaseChip>
      </div>
    </div>
    <!-- Duration -->
    <div class="filter-group">
      <span class="filter-group__label">{{ t('explore.filter_duration') }}</span>
      <div class="filter-group__chips">
        <BaseChip small :active="filters.duration === 'all'" @click="set('duration','all')">{{ t('explore.all') }}</BaseChip>
        <BaseChip v-for="o in durationOptions" :key="o.id" small :active="filters.duration === o.id" @click="set('duration', o.id)">{{ t(o.key) }}</BaseChip>
      </div>
    </div>
    <!-- Sort -->
    <div class="filter-group">
      <span class="filter-group__label">{{ t('explore.filter_sort') }}</span>
      <div class="filter-group__chips">
        <BaseChip v-for="o in sortOptions" :key="o.id" small :active="filters.sort === o.id" @click="set('sort', o.id)">{{ t(o.key) }}</BaseChip>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'
import BaseChip from '@/components/base/BaseChip.vue'
const { t } = useI18n()
const props = defineProps({ filters: Object })
const emit  = defineEmits(['update:filters'])

const typeOptions     = [
  { id: 'meditation',  key: 'explore.type_meditation' },
  { id: 'breathing',   key: 'explore.type_breathing'  },
  { id: 'sleep-story', key: 'explore.type_sleep'      },
  { id: 'soundscape',  key: 'explore.type_soundscape' },
  { id: 'motivational',key: 'explore.type_motivational'}
]
const durationOptions = [
  { id: '1-5',   key: 'explore.dur_1_5'   },
  { id: '5-10',  key: 'explore.dur_5_10'  },
  { id: '10-20', key: 'explore.dur_10_20' },
  { id: '20+',   key: 'explore.dur_20plus'}
]
const sortOptions     = [
  { id: 'popular',     key: 'explore.sort_popular'     },
  { id: 'newest',      key: 'explore.sort_newest'      },
  { id: 'recommended', key: 'explore.sort_recommended' }
]

function set(key, val) {
  emit('update:filters', { ...props.filters, [key]: val })
}
</script>

<style scoped>
.filter-panel { display: flex; flex-direction: column; gap: 20px; }
.filter-group {}
.filter-group__label {
  display: block; font-size: 11px; font-weight: 700; color: var(--text-muted);
  text-transform: uppercase; letter-spacing: 2px; margin-bottom: 10px;
  border-left: 3px solid var(--sage-400); padding-left: 10px;
}
.filter-group__options { display: flex; flex-direction: column; gap: 4px; }
.filter-opt {
  display: flex; align-items: center; gap: 8px;
  padding: 8px 10px; border-radius: var(--radius-sm);
  cursor: pointer; font-size: 14px; color: var(--text-secondary);
  transition: all var(--duration-fast);
  border: 1.5px solid transparent;
}
.filter-opt:hover { color: var(--ink-900); background: var(--parchment-2); }
.filter-opt--active { color: var(--sage-700); background: var(--sage-50); border-color: var(--sage-200); font-weight: 600; }
.filter-opt__dot {
  width: 7px; height: 7px; border-radius: 2px;
  background: var(--ink-200); flex-shrink: 0; transition: background var(--duration-fast);
}
.filter-opt--active .filter-opt__dot { background: var(--sage-500); }
</style>
