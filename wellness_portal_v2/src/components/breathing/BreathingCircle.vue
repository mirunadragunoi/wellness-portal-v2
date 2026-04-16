<template>
  <div class="breathing-wrap">
    <!-- Outer rings -->
    <div class="ring ring--3" :style="ringStyle" />
    <div class="ring ring--2" :style="ringStyle" />
    <div class="ring ring--1" :style="ringStyle" />

    <!-- Main orb -->
    <div class="orb" :class="orbClass" :style="orbStyle">
      <Transition name="phase" mode="out-in">
        <div :key="phase?.phase" class="orb__content">
          <span class="orb__label">{{ phase?.label || 'Ready' }}</span>
          <span class="orb__count">{{ isRunning ? countdown : '' }}</span>
        </div>
      </Transition>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  phase:     { type: Object, default: null },
  countdown: { type: Number, default: 0 },
  isRunning: { type: Boolean, default: false },
  color:     { type: String, default: 'var(--sage-500)' }
})

const orbClass = computed(() => ({
  'orb--idle':   !props.isRunning,
  'orb--inhale': props.phase?.phase === 'inhale',
  'orb--hold':   props.phase?.phase === 'hold',
  'orb--exhale': props.phase?.phase === 'exhale',
  'orb--pause':  props.phase?.phase === 'pause'
}))

const orbStyle = computed(() => ({
  '--orb-color': props.color,
  '--orb-color-light': props.color + '33'
}))

const ringStyle = computed(() => ({ '--ring-color': props.color }))
</script>

<style scoped>
.breathing-circle { display: flex; flex-direction: column; align-items: center; gap: 24px; }
.circle-wrap { position: relative; display: flex; align-items: center; justify-content: center; }
.circle-ring {
  position: absolute; border-radius: 50%;
  border: 1.5px solid rgba(255,255,255,0.15);
}
.circle-ring--outer { width: 300px; height: 300px; }
.circle-ring--mid   { width: 260px; height: 260px; opacity: 0.6; }
.circle-main {
  width: 220px; height: 220px; border-radius: 50%;
  background: rgba(255,255,255,0.07);
  border: 2px solid rgba(255,255,255,0.2);
  display: flex; flex-direction: column;
  align-items: center; justify-content: center; gap: 8px;
  transition: transform 0.1s linear;
  position: relative;
}
.circle-main::after {
  content: ''; position: absolute; inset: 12px; border-radius: 50%;
  background: radial-gradient(circle at 40% 40%, rgba(71,127,60,0.25) 0%, transparent 70%);
}
.circle-phase {
  font-family: var(--font-display); font-size: 20px; font-style: italic;
  color: rgba(255,255,255,0.9); position: relative; z-index: 1;
}
.circle-count {
  font-family: var(--font-display); font-size: 44px; font-weight: 300;
  color: white; line-height: 1; position: relative; z-index: 1;
}
.circle-hint { font-size: 12px; color: rgba(255,255,255,0.4); text-transform: uppercase; letter-spacing: 1.5px; position: relative; z-index: 1; }
.circle-idle { animation: breathe 8s ease-in-out infinite; }
@keyframes breathe { 0%,100%{transform:scale(1);} 50%{transform:scale(1.08);} }
</style>
