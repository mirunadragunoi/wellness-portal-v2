<template>
  <div class="recommended">
    <div class="recommended__header">
      <h2 class="recommended__title">{{ t('home.recommended_title') }}</h2>
      <RouterLink to="/explore" class="recommended__see-all">
        {{ t('home.see_all') }}
        <Icon icon="lucide:chevron-right" class="app-icon app-icon--xs" aria-hidden="true" />
      </RouterLink>
    </div>
    <div class="recommended__scroll">
      <div class="recommended__track">
        <div
          v-for="session in recommended"
          :key="session.id"
          class="session-card"
          @click="goToSession(session)"
        >
          <div class="session-card__img" :style="{ background: session.thumbnailGradient }">
            <span class="session-card__badge" :style="getBadgeStyle(session.category)">
              {{ session.category }}
            </span>
            <button type="button" class="session-card__play" aria-label="Play">
              <Icon icon="lucide:play" class="app-icon app-icon--sm" />
            </button>
            <span class="session-card__type-icon">
              <Icon :icon="sessionTypeIcon(session.type)" class="app-icon app-icon--sm" />
            </span>
          </div>
          <div class="session-card__body">
            <h4 class="session-card__title">{{ session.title }}</h4>
            <div class="session-card__meta">
              <span>{{ Math.round(session.duration / 60) }} {{ t('explore.min') }}</span>
              <span>·</span>
              <span>{{ session.level }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'
import { useRouter } from 'vue-router'
import { useRecommendations } from '@/composables/useGreeting'
import { usePlayerStore } from '@/stores/player'
import { sessionTypeIcon } from '@/constants/appIcons'

const { t }    = useI18n()
const router   = useRouter()
const player   = usePlayerStore()
const { recommended } = useRecommendations(8)

const categoryColors = {
  stress:      { bg: 'var(--sage-100)', color: '#1e40af' },
  anxiety:     { bg: '#f0f7ee', color: 'var(--sage-700)' },
  sleep:       { bg: '#e0e7ff', color: '#3730a3' },
  focus:       { bg: 'var(--parchment-2)', color: 'var(--ink-700)' },
  mindfulness: { bg: '#dcfce7', color: '#166534' },
  energy:      { bg: '#fef3c7', color: '#92400e' }
}

function getBadgeStyle(cat) {
  const c = categoryColors[cat] || { bg: '#f1f5f9', color: '#475569' }
  return { background: c.bg, color: c.color }
}

function goToSession(session) {
  player.loadSession(session)
  router.push({ name: 'session', params: { id: session.id } })
}
</script>

<style scoped>
.recommended { display: flex; flex-direction: column; gap: 14px; }
.recommended__header { display: flex; align-items: center; justify-content: space-between; }
.recommended__title { font-family: var(--font-display); font-size: 20px; font-weight: 400; color: var(--ink-900); }
.recommended__see-all {
  font-size: 13px; font-weight: 600; color: var(--sage-600); text-decoration: none;
  border-bottom: 1.5px solid var(--sage-300); padding-bottom: 1px; transition: color var(--duration-fast);
}
.recommended__see-all:hover { color: var(--sage-700); }
.recommended__list { display: flex; flex-direction: column; gap: 10px; }
.rec-card {
  display: flex; align-items: center; gap: 14px;
  background: var(--bg-surface); border: 2px solid var(--ink-100);
  border-radius: var(--radius-lg); padding: 14px; cursor: pointer;
  text-decoration: none; transition: all var(--duration-fast);
  box-shadow: 2px 2px 0 var(--ink-100);
}
.rec-card:hover { border-color: var(--sage-300); transform: translate(-2px,-2px); box-shadow: 4px 4px 0 var(--sage-100); }
.rec-card__thumb {
  width: 52px; height: 52px; border-radius: var(--radius-sm); flex-shrink: 0;
  display: flex; align-items: center; justify-content: center;
  border: 1.5px solid var(--ink-100);
}
.rec-card__info { flex: 1; min-width: 0; }
.rec-card__title { font-family: var(--font-display); font-size: 16px; font-weight: 400; color: var(--ink-900); margin-bottom: 4px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.rec-card__meta  { display: flex; gap: 10px; }
.meta-item { display: flex; align-items: center; gap: 4px; font-size: 12px; color: var(--text-muted); font-weight: 500; }
.rec-card__cat { flex-shrink: 0; font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; padding: 3px 8px; border-radius: var(--radius-sm); }
.rec-card__play {
  width: 36px; height: 36px; border-radius: var(--radius-sm); flex-shrink: 0;
  display: flex; align-items: center; justify-content: center;
  background: var(--ink-900); color: var(--parchment); border: none; cursor: pointer;
  transition: all var(--duration-fast); box-shadow: 2px 2px 0 var(--ink-700);
}
.rec-card__play:hover { background: var(--sage-500); box-shadow: 2px 2px 0 var(--sage-700); }
</style>
