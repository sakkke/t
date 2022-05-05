<script setup lang="ts">
import { computed, ref } from 'vue'

const audio = new Audio(new URL('../assets/magic-11238.mp3', import.meta.url).href)
audio.loop = true

const decreaseUntilZeroBy = (n: number) => () => {
  const limit = 0

  if (time.value - n < limit) {
    time.value = limit
  } else {
    time.value -= n
  }
}
const increaseUntilSixtyBy = (n: number) => () => {
  const limit = 3600

  if (time.value + n > limit) {
    time.value = limit
  } else {
    time.value += n
  }
}

const props = withDefaults(defineProps<{
  active?: boolean
  time?: number
}>(), {
  active: false,
  time: 60,
})

const active = ref(props.active)
const time = ref(props.time)
const timer = ref(0)

const finished = computed(() => time.value === 0)
const limited = computed(() => time.value === 3600)
const seconds = computed(() => `${time.value % 60}`.padStart(2, '0'))
const minutes = computed(() => `${Math.floor(time.value / 60)}`)

const decreaseFiveMinutes = decreaseUntilZeroBy(300)
const decreaseMinute = decreaseUntilZeroBy(60)
const decreaseSecond = decreaseUntilZeroBy(1)
const decreaseTenMinutes = decreaseUntilZeroBy(600)
const increaseFiveMinutes = increaseUntilSixtyBy(300)
const increaseMinute = increaseUntilSixtyBy(60)
const increaseSecond = increaseUntilSixtyBy(1)
const increaseTenMinutes = increaseUntilSixtyBy(600)
const initTime = () => {
  time.value = props.time
}
const stopAudio = () => {
  audio.pause()
  audio.currentTime = 0
}
const toggleTimer = () => {
  active.value = !active.value

  if (active.value) {
    timer.value = setInterval(() => {
      if (time.value <= 0) {
        audio.play()
        toggleTimer()
        return
      }

      time.value--
    }, 1000)
  } else {
    clearInterval(timer.value)
  }
}

defineExpose({
  decreaseMinute,
  decreaseSecond,
  finished,
  increaseMinute,
  increaseSecond,
  initTime,
  stopAudio,
  time,
  toggleTimer,
})
</script>

<template>
  <div :class="{ active, finished, limited }">
    <div @click="toggleTimer" class="timer-time">
      <span class="icon">
        {{ active ? '&#xe1c4;' : '&#xef71;' }}
      </span>
      <span class="time">{{ minutes }}:{{ seconds }}</span>
    </div>
    <div class="p-container">
      <p class="timer-desc">
        Pressing the space key or clicking/touching the time toggles the timer start/stop.
      </p>
    </div>
    <div class="buttons-container">
      <b>Control</b>
      <div class="buttons">
        <button :disabled="finished" @click="decreaseMinute">-1:00</button>
        <button :disabled="finished" @click="decreaseSecond">-0:01</button>
        <button :disabled="finished" @click="increaseSecond">+0:01</button>
        <button :disabled="finished" @click="increaseMinute">+1:00</button>
      </div>
      <div class="buttons">
        <button :disabled="finished" @click="decreaseTenMinutes">-10:00</button>
        <button :disabled="finished" @click="decreaseFiveMinutes">-5:00</button>
        <button :disabled="finished" @click="increaseFiveMinutes">+5:00</button>
        <button :disabled="finished" @click="increaseTenMinutes">+10:00</button>
      </div>
    </div>
    <div class="buttons-container">
      <b>Presets</b>
      <div class="buttons">
        <button :disabled="finished" @click="time = 60">1:00</button>
        <button :disabled="finished" @click="time = 120">2:00</button>
        <button :disabled="finished" @click="time = 180">3:00</button>
        <button :disabled="finished" @click="time = 240">4:00</button>
        <button :disabled="finished" @click="time = 300">5:00</button>
      </div>
      <div class="buttons">
        <button :disabled="finished" @click="time = 360">6:00</button>
        <button :disabled="finished" @click="time = 420">7:00</button>
        <button :disabled="finished" @click="time = 480">8:00</button>
        <button :disabled="finished" @click="time = 540">9:00</button>
        <button :disabled="finished" @click="time = 600">10:00</button>
      </div>
      <div class="buttons">
        <button :disabled="finished" @click="time = 1200">20:00</button>
        <button :disabled="finished" @click="time = 1800">30:00</button>
        <button :disabled="finished" @click="time = 2400">40:00</button>
        <button :disabled="finished" @click="time = 3000">50:00</button>
        <button :disabled="finished" @click="time = 3600">60:00</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
:not(.active) .timer-time .icon {
  color: var(--red-9);
}

.active .timer-time .icon {
  color: var(--green-9);
}

.buttons {
  display: flex;
  flex-wrap: wrap;
  gap: .5rem;
  justify-content: center;
}

.buttons button {
  background-color: var(--gray-2);
  border: var(--border-size-1) solid var(--gray-5);
  border-radius: var(--radius-5);
}

.buttons button:hover {
  background-color: var(--gray-3);
}

.buttons-container {
  display: flex;
  flex-direction: column;
  gap: .5rem;
  margin-top: 1rem;
}

.finished .timer-time .time {
  color: var(--red-9);
}

.limited .timer-time .time {
  color: var(--blue-9);
}

.timer-desc {
  color: var(--gray-8);
  font-size: var(--font-size-0);
}

.timer-time {
  display: flex;
  font-size: 25vmin;
  justify-content: center;
}

.timer-time .time {
  font-weight: var(--font-weight-9);
}
</style>
