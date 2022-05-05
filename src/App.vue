<script setup lang="ts">
import Timer from './components/Timer.vue'
import { ref } from 'vue'
import '@fontsource/material-icons-outlined'
import 'open-props/style'
import 'open-props/normalize'

const timer = ref()

const handleClickOrKeydown = () => {
  if (!timer.value.finished) return
  timer.value.stopAudio()
  timer.value.initTime()
}

window.addEventListener('click', e => {
  if ((e.target as HTMLElement).nodeName === 'BUTTON') return
  handleClickOrKeydown()
})

window.addEventListener('keydown', e => {
  switch (e.key) {
    case ' ':
      timer.value.toggleTimer()
      break

    case '0':
      timer.value.time = 600
      break

    case '1':
    case '2':
    case '3':
    case '4':
    case '5':
    case '6':
    case '7':
    case '8':
    case '9':
      timer.value.time = 60 * Number(e.key)
      break

    case 'ArrowDown':
      timer.value.decreaseMinute()
      break

    case 'ArrowLeft':
      timer.value.decreaseSecond()
      break

    case 'ArrowRight':
      timer.value.increaseSecond()
      break

    case 'ArrowUp':
      timer.value.increaseMinute()
      break

    default:
      handleClickOrKeydown()
  }
})
</script>

<template>
  <header>
    <h1>
      <a href="#">
        <span class="icon">&#xe425;</span>
        <span>t (timer)</span>
      </a>
    </h1>
  </header>
  <div>
    <div class="timer">
      <Timer ref="timer" />
    </div>
  </div>
  <footer>
    <div class="p-container">
      <p>
        <span class="icon">&#xe405;</span>
        <span>
          Music by <a href="https://pixabay.com/users/joo_yy-24508386/?tab=audio&amp;utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=audio&amp;utm_content=11238">joo_yy</a> from <a href="https://pixabay.com/music/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=music&amp;utm_content=11238">Pixabay</a>
        </span>
      </p>
    </div>
    <div class="p-container">
      <p>
        <span class="icon">&#xe815;</span>
        <span>
          App by <a href="https://twitter.com/SakkkeDev">@SakkkeDev</a>
        </span>
      </p>
    </div>
  </footer>
</template>

<style>
footer p {
  display: flex;
}

h1 a {
  background-image: var(--gradient-1);
  background-clip: text;
  -webkit-background-clip: text;
  color: transparent;
  display: inline-flex;
  text-decoration: none;
}

#app {
  align-items: center;
  background-color: var(--gray-0);
  color: var(--gray-9);
  display: flex;
  flex-direction: column;
  font-family: Inter, var(--font-sans);
  gap: 1rem;
  justify-content: center;
  min-height: 100vh;
}

.icon {
  font-family: 'Material Icons Outlined';
}

.p-container {
  display: flex;
  justify-content: center;
}

.timer {
  text-align: center;
}
</style>
