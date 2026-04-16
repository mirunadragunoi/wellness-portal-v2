<template>
  <div class="audio-player">
    <!-- Background blurred cover -->
    <div class="audio-player__bg" :style="{ background: session?.thumbnailGradient }" />
    <div class="audio-player__overlay" />

    <div class="audio-player__inner">
      <!-- Cover art -->
      <div class="audio-player__cover" :style="{ background: session?.thumbnailGradient }">
        <Icon :icon="coverIcon" class="audio-player__cover-icon app-icon app-icon--3xl" />
      </div>

      <!-- Track info -->
      <div class="audio-player__info">
        <h2 class="audio-player__title">{{ session?.title }}</h2>
        <p class="audio-player__meta">{{ session?.category }} · {{ session?.type }}</p>
      </div>

      <!-- Progress bar -->
      <div class="audio-player__progress" @click="onProgressClick" ref="progressRef">
        <div class="progress-track">
          <div class="progress-fill" :style="{ width: playerStore.progress + '%' }" />
          <div class="progress-thumb" :style="{ left: playerStore.progress + '%' }" />
        </div>
        <div class="progress-times">
          <span>{{ playerStore.formattedCurrent }}</span>
          <span>{{ playerStore.formattedDuration }}</span>
        </div>
      </div>

      <!-- Controls -->
      <div class="audio-player__controls">
        <button class="ctrl-btn ctrl-btn--sm" @click="player.skipBack()" title="Back 15s">
          ⟨15
        </button>
        <button type="button" class="ctrl-btn ctrl-btn--play" @click="toggle" :aria-label="playerStore.isPlaying ? 'Pause' : 'Play'">
          <Icon :icon="playerStore.isPlaying ? 'lucide:pause' : 'lucide:play'" class="app-icon app-icon--xl" />
        </button>
        <button class="ctrl-btn ctrl-btn--sm" @click="player.skipForward()" title="Forward 15s">
          15⟩
        </button>
      </div>

      <!-- Volume -->
      <div class="audio-player__volume">
        <Icon :icon="volumeIcon" class="volume-icon app-icon app-icon--md app-icon--muted" aria-hidden="true" />
        <input
          class="volume-slider"
          type="range" min="0" max="1" step="0.01"
          :value="playerStore.volume"
          @input="player.setVolume(+$event.target.value)"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { usePlayerStore } from '@/stores/player'
import { useAudioPlayer } from '@/composables/useAudioPlayer'
import { sessionTypeIcon } from '@/constants/appIcons'

const props = defineProps({ session: Object })
const playerStore  = usePlayerStore()
const { toggle, seek } = useAudioPlayer()
const player       = usePlayerStore()
const progressRef  = ref(null)

const coverIcon = computed(() => sessionTypeIcon(props.session?.type))

const volumeIcon = computed(() => {
  const v = playerStore.volume
  if (v > 0.5) return 'lucide:volume-2'
  if (v > 0) return 'lucide:volume-1'
  return 'lucide:volume-x'
})

function onProgressClick(e) {
  const rect = progressRef.value?.getBoundingClientRect()
  if (!rect) return
  const pct = (e.clientX - rect.left) / rect.width
  seek(pct * playerStore.duration)
}
</script>

<style scoped>
.player {
  min-height: var(--app-min-height); display: flex; flex-direction: column;
  background: var(--ink-900); position: relative; overflow: hidden;
}
.player__bg {
  position: absolute; inset: 0; background-size: cover; background-position: center;
  opacity: 0.12; filter: blur(20px); transform: scale(1.1);
}
.player__inner {
  position: relative; z-index: 1;
  flex: 1; display: flex; flex-direction: column; align-items: center;
  justify-content: space-between; padding: 60px var(--container-pad) 40px;
  max-width: 520px; margin: 0 auto; width: 100%;
}
.player__thumb {
  width: clamp(160px, 40vw, 240px); aspect-ratio: 1;
  border-radius: var(--radius-lg); background-size: cover; background-position: center;
  border: 3px solid rgba(255,255,255,0.15);
  box-shadow: 8px 8px 0 rgba(0,0,0,0.4);
  display: flex; align-items: center; justify-content: center;
}
.player__info { text-align: center; }
.player__category {
  display: inline-block; padding: 4px 12px; margin-bottom: 12px;
  border: 1.5px solid rgba(255,255,255,0.2); border-radius: var(--radius-sm);
  font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 2px; color: rgba(255,255,255,0.6);
}
.player__title { font-family: var(--font-display); font-size: 28px; font-weight: 300; color: white; margin-bottom: 6px; }
.player__author { font-size: 14px; color: rgba(255,255,255,0.5); }
.player__timeline { width: 100%; display: flex; flex-direction: column; gap: 8px; }
.player__progress { width: 100%; -webkit-appearance: none; height: 4px; border-radius: 2px; cursor: pointer; background: rgba(255,255,255,0.15); outline: none; }
.player__progress::-webkit-slider-thumb { -webkit-appearance: none; width: 14px; height: 14px; border-radius: var(--radius-sm); background: var(--sage-400); cursor: pointer; }
.player__times { display: flex; justify-content: space-between; font-size: 12px; color: rgba(255,255,255,0.4); font-weight: 600; }
.player__controls { display: flex; align-items: center; gap: 16px; }
.ctrl-btn {
  background: none; border: none; cursor: pointer; color: rgba(255,255,255,0.6);
  padding: 8px; border-radius: var(--radius-sm); transition: all var(--duration-fast);
  display: flex; align-items: center; justify-content: center;
}
.ctrl-btn:hover { color: white; background: rgba(255,255,255,0.08); }
.ctrl-btn--play {
  width: 60px; height: 60px; border-radius: var(--radius);
  background: var(--sage-500); color: white; border: 2px solid var(--sage-400);
  box-shadow: 3px 3px 0 var(--sage-700);
}
.ctrl-btn--play:hover { background: var(--sage-400); transform: translate(-1px,-1px); box-shadow: 4px 4px 0 var(--sage-700); }
</style>
