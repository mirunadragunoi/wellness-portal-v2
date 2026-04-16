<template>
  <div class="category-chips no-scrollbar">
    <BaseChip
      :active="modelValue === 'all'"
      @click="emit('update:modelValue', 'all')"
    >
      {{ t('explore.all') }}
    </BaseChip>
    <BaseChip
      v-for="cat in categories"
      :key="cat.id"
      :active="modelValue === cat.id"
      :icon="cat.icon"
      :color="cat.color"
      @click="emit('update:modelValue', cat.id)"
    >
      {{ cat.label }}
    </BaseChip>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'
import { categories } from '@/data/phrases'
import BaseChip from '@/components/base/BaseChip.vue'
const { t } = useI18n()
defineProps({ modelValue: { type: String, default: 'all' } })
const emit = defineEmits(['update:modelValue'])
</script>

<style scoped>
.cat-chips {
  display: flex; gap: 0; overflow-x: auto;
  border-bottom: 2px solid var(--ink-200);
  background: var(--bg-surface);
  padding: 0 var(--container-pad);
  margin: 0 calc(-1 * var(--container-pad));
}
.cat-chip {
  display: flex; align-items: center; gap: 6px;
  padding: 12px 18px; border-bottom: 3px solid transparent;
  font-size: 14px; font-weight: 500; color: var(--text-secondary);
  background: none; border-left: none; border-right: none; border-top: none;
  cursor: pointer; white-space: nowrap; margin-bottom: -2px;
  transition: all var(--duration-fast); font-family: var(--font-body);
}
.cat-chip:hover { color: var(--ink-900); }
.cat-chip--active { color: var(--ink-900); font-weight: 700; border-bottom-color: var(--sage-500); }
.cat-chip__icon { opacity: 0.7; }
.cat-chip--active .cat-chip__icon { opacity: 1; color: var(--sage-500); }
</style>
