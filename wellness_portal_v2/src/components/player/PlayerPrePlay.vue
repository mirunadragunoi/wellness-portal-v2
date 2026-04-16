<template>
  <div class="pre-play">
    <div class="pre-play__cover" :style="{ background: session?.thumbnailGradient }">
      <Icon :icon="coverIcon" class="pre-play__type-icon app-icon app-icon--3xl" />
    </div>

    <div class="pre-play__content">
      <span class="pre-play__category-badge">{{ session?.category }}</span>
      <h1 class="pre-play__title">{{ session?.title }}</h1>
      <p class="pre-play__meta">
        {{ session?.type }} · {{ durationLabel }} · {{ session?.level }}
      </p>
      <p class="pre-play__desc">{{ session?.description }}</p>

      <div class="pre-play__actions">
        <button type="button" class="pre-play__play-btn" @click="emit('play')">
          <Icon icon="lucide:play" class="pre-play__play-icon app-icon app-icon--md" />
          {{ t('player.play') }}
        </button>
        <button
          class="pre-play__fav-btn"
          :class="{ 'pre-play__fav-btn--active': isFavorite }"
          @click="emit('favorite')"
          :title="isFavorite ? t('player.remove_favorite') : t('player.add_favorite')"
        >
          <Icon :icon="isFavorite ? 'mdi:heart' : 'lucide:heart'" class="app-icon app-icon--md" />
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useI18n } from 'vue-i18n'
import { useProgressStore } from '@/stores/progress'
import { sessionTypeIcon } from '@/constants/appIcons'

const { t } = useI18n()
const props = defineProps({ session: Object })
const emit  = defineEmits(['play', 'favorite'])
const progressStore = useProgressStore()

const isFavorite = computed(() => progressStore.isFavorite(props.session?.id))
const coverIcon = computed(() => sessionTypeIcon(props.session?.type))
const durationLabel = computed(() => {
  const m = Math.round((props.session?.duration || 0) / 60)
  return `${m} ${t('explore.min')}`
})
</script>

<style scoped>
.pre-play {
  min-height: var(--app-min-height); display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  background: var(--ink-900); padding: 40px var(--container-pad);
}
.pre-play__card {
  width: 100%; max-width: 480px;
  background: var(--parchment); border: 2px solid var(--ink-300);
  border-radius: var(--radius-xl); overflow: hidden;
  box-shadow: 8px 8px 0 var(--sage-700);
}
.pre-play__cover {
  height: 220px; background-size: cover; background-position: center;
  display: flex; align-items: flex-end; padding: 20px;
}
.pre-play__cat {
  padding: 5px 12px; background: white; border-radius: var(--radius-sm);
  font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.5px;
  color: var(--ink-900);
}
.pre-play__body { padding: 28px; }
.pre-play__title { font-family: var(--font-display); font-size: 28px; font-weight: 300; color: var(--ink-900); margin-bottom: 12px; line-height: 1.2; }
.pre-play__meta  { display: flex; gap: 16px; margin-bottom: 20px; }
.meta-chip { display: flex; align-items: center; gap: 6px; font-size: 13px; color: var(--text-secondary); font-weight: 500; }
.pre-play__desc { font-size: 15px; color: var(--text-secondary); line-height: 1.7; margin-bottom: 28px; }
.pre-play__actions { display: flex; gap: 12px; }
.btn-start {
  flex: 1; display: flex; align-items: center; justify-content: center; gap: 8px;
  padding: 14px 24px; border-radius: var(--radius-sm); cursor: pointer;
  background: var(--ink-900); color: var(--parchment);
  border: 2px solid var(--ink-900); font-size: 15px; font-weight: 700;
  font-family: var(--font-body); box-shadow: 3px 3px 0 var(--sage-500);
  transition: all var(--duration-fast);
}
.btn-start:hover { background: var(--sage-500); border-color: var(--sage-500); box-shadow: 3px 3px 0 var(--sage-700); }
.btn-fav {
  width: 52px; height: 52px; display: flex; align-items: center; justify-content: center;
  border: 2px solid var(--ink-200); border-radius: var(--radius-sm);
  background: var(--parchment-2); color: var(--text-secondary); cursor: pointer;
  transition: all var(--duration-fast);
}
.btn-fav:hover { border-color: var(--terracotta); color: var(--terracotta); }
.btn-fav--active { color: var(--terracotta); border-color: var(--terracotta); background: #faf0eb; }
</style>
