<template>
  <div class="session-card" @click="emit('play', session)">
    <!-- Thumbnail -->
    <div class="session-card__img" :style="{ background: session.thumbnailGradient }">
      <span class="session-card__badge" :style="badgeStyle">{{ session.category }}</span>
      <div class="session-card__play-overlay">
        <button type="button" class="play-btn" aria-label="Play" @click.stop="emit('play', session)">
          <Icon icon="lucide:play" class="app-icon app-icon--lg" />
        </button>
      </div>
      <span class="session-card__type">
        <Icon :icon="sessionTypeIcon(session.type)" class="app-icon app-icon--sm" />
      </span>
    </div>

    <!-- Body -->
    <div class="session-card__body">
      <h4 class="session-card__title">{{ session.title }}</h4>
      <div class="session-card__meta">
        <span class="session-card__duration">{{ durationLabel }}</span>
        <span class="session-card__dot">·</span>
        <span class="session-card__level">{{ session.level }}</span>
      </div>
    </div>

    <!-- Favorite -->
    <button
      class="session-card__fav"
      :class="{ 'session-card__fav--active': isFavorite }"
      @click.stop="emit('favorite', session.id)"
      :aria-label="t('player.add_favorite')"
    >
      <Icon
        :icon="isFavorite ? 'mdi:heart' : 'lucide:heart'"
        class="session-card__fav-icon app-icon app-icon--md"
      />
    </button>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useI18n } from 'vue-i18n'
import { useProgressStore } from '@/stores/progress'
import { sessionTypeIcon } from '@/constants/appIcons'

const { t } = useI18n()
const props = defineProps({ session: { type: Object, required: true } })
const emit  = defineEmits(['play', 'favorite'])

const progressStore = useProgressStore()
const isFavorite = computed(() => progressStore.isFavorite(props.session.id))

const categoryColors = {
  stress:      { bg: 'var(--sage-100)', color: '#1e40af' },
  anxiety:     { bg: '#f0f7ee', color: 'var(--sage-700)' },
  sleep:       { bg: '#e0e7ff', color: '#3730a3' },
  focus:       { bg: 'var(--parchment-2)', color: 'var(--ink-700)' },
  mindfulness: { bg: '#dcfce7', color: '#166534' },
  energy:      { bg: '#fef3c7', color: '#92400e' }
}
const badgeStyle = computed(() => {
  const c = categoryColors[props.session.category] || { bg: '#f1f5f9', color: '#475569' }
  return { background: c.bg, color: c.color }
})

const durationLabel = computed(() => {
  const m = Math.round(props.session.duration / 60)
  return `${m} ${t('explore.min')}`
})
</script>

<style scoped>
.session-card {
  background: var(--bg-surface);
  border: 2px solid var(--ink-200); border-radius: var(--radius-lg);
  overflow: hidden; display: flex; flex-direction: column;
  cursor: pointer; transition: all var(--duration-normal);
  box-shadow: 3px 3px 0 var(--ink-100);
}
.session-card:hover { transform: translate(-2px,-3px); box-shadow: 6px 6px 0 var(--sage-200); border-color: var(--sage-300); }

.session-card__thumb {
  height: 120px; position: relative; overflow: hidden;
  background-size: cover; background-position: center;
  display: flex; align-items: flex-end;
}
.session-card__overlay {
  position: absolute; inset: 0;
  background: linear-gradient(to top, rgba(28,26,22,0.55), transparent 60%);
}
.session-card__cat {
  position: absolute; top: 10px; left: 10px;
  padding: 4px 10px; border-radius: var(--radius-sm);
  font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.5px;
}
.session-card__play {
  position: absolute; bottom: 10px; right: 10px;
  width: 36px; height: 36px; border-radius: var(--radius-sm);
  background: white; border: none; cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  color: var(--ink-900); box-shadow: 2px 2px 0 var(--ink-300);
  transition: all var(--duration-fast);
}
.session-card__play:hover { background: var(--sage-500); color: white; box-shadow: 2px 2px 0 var(--sage-700); }

.session-card__body { padding: 16px; flex: 1; display: flex; flex-direction: column; gap: 10px; }
.session-card__title {
  font-family: var(--font-display); font-size: 18px; font-weight: 400;
  color: var(--ink-900); line-height: 1.3;
}
.session-card__meta { display: flex; align-items: center; gap: 12px; margin-top: auto; }
.meta-item { display: flex; align-items: center; gap: 4px; font-size: 12px; color: var(--text-muted); font-weight: 500; }
.session-card__fav {
  margin-left: auto; background: none; border: none; cursor: pointer;
  color: var(--text-muted); padding: 4px; border-radius: var(--radius-sm);
  transition: color var(--duration-fast);
}
.session-card__fav--active { color: var(--terracotta); }
.session-card__fav:hover { color: var(--terracotta); }
</style>
