<template>
  <div class="post-session">
    <div class="post-session__check">
      <Icon icon="lucide:check" class="app-icon app-icon--2xl" />
    </div>
    <h2 class="post-session__title">{{ t('player.completed_title') }}</h2>
    <p class="post-session__body">{{ t('player.completed_body', { title: session?.title }) }}</p>

    <!-- Streak update -->
    <div class="post-session__streak">
      <Icon icon="lucide:flame" class="streak-fire app-icon app-icon--xl" aria-hidden="true" />
      <div>
        <p class="streak-value">{{ progressStore.streakDays }} {{ t('common.days') }}</p>
        <p class="streak-label">{{ t('player.streak_updated') }}</p>
      </div>
    </div>

    <!-- Post mood -->
    <div class="post-session__mood">
      <p class="post-session__mood-q">{{ t('player.post_mood_prompt') }}</p>
      <div class="mood-options">
        <button v-for="m in moodOptions" :key="m.id" class="mood-btn" @click="checkMood(m.id)">
          <Icon :icon="m.icon" class="app-icon app-icon--md app-icon--muted" />
          <span>{{ t(`home.mood_${m.id}`) }}</span>
        </button>
      </div>
    </div>

    <!-- Suggestions -->
    <div class="post-session__suggestions">
      <p class="post-session__suggestions-title">{{ t('player.suggestions_title') }}</p>
      <div class="suggestions-list">
        <div
          v-for="s in suggestions" :key="s.id"
          class="suggestion-card"
          @click="router.push({ name: 'session', params: { id: s.id } })"
        >
          <div class="suggestion-card__img" :style="{ background: s.thumbnailGradient }" />
          <div class="suggestion-card__info">
            <p class="suggestion-card__title">{{ s.title }}</p>
            <p class="suggestion-card__meta">{{ Math.round(s.duration / 60) }} {{ t('explore.min') }}</p>
          </div>
        </div>
      </div>
    </div>

    <RouterLink to="/home" class="back-btn">{{ t('player.back_home') }}</RouterLink>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useI18n } from 'vue-i18n'
import { useRouter } from 'vue-router'
import { useProgressStore } from '@/stores/progress'
import { useMoodStore, moodOptions } from '@/stores/mood'
import { useUIStore } from '@/stores/ui'
import { sessions } from '@/data/sessions'

const { t }          = useI18n()
const router         = useRouter()
const progressStore  = useProgressStore()
const moodStore      = useMoodStore()
const uiStore        = useUIStore()

const props = defineProps({ session: Object })

const suggestions = computed(() =>
  sessions
    .filter(s => s.id !== props.session?.id && s.category === props.session?.category)
    .slice(0, 3)
)

function checkMood(id) {
  moodStore.checkIn(id)
  uiStore.showToast(t('home.mood_saved'), 'success')
}
</script>

<style scoped>
.post-session {
  min-height: var(--app-min-height); display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  background: var(--ink-900); padding: 40px var(--container-pad); text-align: center;
}
.post-session__check {
  width: 72px; height: 72px; border-radius: var(--radius);
  background: var(--sage-500); border: 3px solid var(--sage-400);
  display: flex; align-items: center; justify-content: center;
  color: white; animation: bounceIn 0.6s var(--ease-bounce);
  box-shadow: 4px 4px 0 var(--sage-700); margin-bottom: 24px;
}
.post-session__title { font-family: var(--font-display); font-size: 36px; font-weight: 300; color: var(--parchment); margin-bottom: 10px; }
.post-session__sub   { font-size: 16px; color: var(--ink-400); max-width: 360px; line-height: 1.6; margin-bottom: 32px; }
.post-session__mood  { margin-bottom: 36px; }
.post-session__mood-label { font-size: 14px; color: var(--ink-400); margin-bottom: 12px; font-weight: 500; }
.mood-btns { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; }
.mood-btn {
  display: flex; flex-direction: column; align-items: center; gap: 5px;
  padding: 12px 18px; border-radius: var(--radius-sm);
  background: rgba(255,255,255,0.06); border: 2px solid rgba(255,255,255,0.1);
  cursor: pointer; transition: all var(--duration-fast); color: var(--ink-300);
}
.mood-btn:hover { border-color: var(--sage-400); color: var(--sage-300); background: rgba(71,127,60,0.12); }
.mood-btn--active { border-color: var(--sage-500); color: var(--sage-300); background: rgba(71,127,60,0.18); }
.mood-btn__label { font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; }
.post-session__streak {
  display: flex; align-items: center; gap: 12px;
  padding: 14px 24px; border: 2px solid rgba(255,255,255,0.1);
  border-radius: var(--radius); margin-bottom: 28px;
  background: rgba(255,255,255,0.04);
}
.streak-info { text-align: left; }
.streak-info__val   { font-family: var(--font-display); font-size: 22px; font-weight: 300; color: var(--parchment); }
.streak-info__label { font-size: 12px; color: var(--ink-400); text-transform: uppercase; letter-spacing: 1px; }
.post-session__actions { display: flex; gap: 12px; flex-wrap: wrap; justify-content: center; }
.btn {
  padding: 13px 28px; border-radius: var(--radius-sm); font-size: 14px; font-weight: 700;
  cursor: pointer; border: 2px solid transparent; text-decoration: none;
  display: inline-flex; align-items: center; gap: 6px; transition: all var(--duration-fast);
  font-family: var(--font-body);
}
.btn--primary { background: var(--sage-500); color: white; border-color: var(--sage-600); box-shadow: 3px 3px 0 var(--sage-700); }
.btn--primary:hover { background: var(--sage-600); transform: translate(-1px,-1px); }
.btn--ghost { background: transparent; color: var(--ink-400); border-color: var(--ink-700); }
.btn--ghost:hover { color: var(--parchment); border-color: var(--ink-400); }
@keyframes bounceIn { 0%{transform:scale(0.5);opacity:0} 60%{transform:scale(1.1)} 100%{transform:scale(1);opacity:1} }
</style>
