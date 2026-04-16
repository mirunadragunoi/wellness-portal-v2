<template>
  <div class="moment-card" @click="goToSession">
    <div class="moment-card__img" :style="{ background: session?.thumbnailGradient }">
      <span class="moment-card__time-label">{{ label }}</span>
      <button type="button" class="moment-card__play">
        <Icon icon="lucide:play" class="app-icon app-icon--sm" aria-hidden="true" />
        {{ t('home.play') }}
      </button>
    </div>
    <div class="moment-card__body">
      <p class="moment-card__sub">{{ t('home.moment_title') }}</p>
      <h3 class="moment-card__title">{{ session?.title }}</h3>
      <p class="moment-card__meta">{{ session?.type }} · {{ session ? Math.round(session.duration / 60) : 0 }} {{ t('explore.min') }}</p>
    </div>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'
import { useRouter } from 'vue-router'
import { useMomentOfDay } from '@/composables/useGreeting'
import { usePlayerStore } from '@/stores/player'

const { t }   = useI18n()
const router  = useRouter()
const player  = usePlayerStore()
const { session, label } = useMomentOfDay()

function goToSession() {
  if (!session.value) return
  player.loadSession(session.value)
  router.push({ name: 'session', params: { id: session.value.id } })
}
</script>

<style scoped>
.moment-card {
  background: var(--bg-surface); border: 2px solid var(--ink-200);
  border-radius: var(--radius-lg); padding: 20px; min-width: 0;
  box-shadow: 3px 3px 0 var(--ink-100);
}
.moment-card__label {
  font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 2px;
  color: var(--sage-500); border-left: 3px solid var(--sage-400); padding-left: 10px; margin-bottom: 14px;
}
.moment-session {
  display: flex; flex-direction: column; gap: 12px;
}
.moment-session__thumb {
  height: 80px; border-radius: var(--radius);
  background-size: cover; background-position: center;
  border: 1.5px solid var(--ink-100);
}
.moment-session__title { font-family: var(--font-display); font-size: 17px; font-weight: 400; color: var(--ink-900); }
.moment-session__meta  { font-size: 13px; color: var(--text-muted); }
.moment-session__btn {
  display: flex; align-items: center; justify-content: center; gap: 6px;
  padding: 10px; border-radius: var(--radius-sm); width: 100%;
  background: var(--ink-900); color: var(--parchment); border: 2px solid var(--ink-900);
  font-size: 13px; font-weight: 700; cursor: pointer; text-decoration: none;
  box-shadow: 2px 2px 0 var(--sage-400); transition: all var(--duration-fast);
}
.moment-session__btn:hover { background: var(--sage-500); border-color: var(--sage-500); }
.moment-empty { font-size: 14px; color: var(--text-muted); }
</style>
