<template>
  <div class="timer">
    <div class="timer__item">
      <span>{{ formatTime() }}</span>
    </div>
    <div class="timer__actions">
      <img class="icon" @click="start" v-if="!isRunning" src="../assets/start.svg" alt="Start icon">
      <img class="icon" @click="stop" v-else src="../assets/pause.svg" alt="Pause icon">
      <img class="icon" @click="reset" src="../assets/reset.svg" alt="Reset icon">
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

defineProps(['time'])
let intervalId = null
let isRunning = ref(false)
let seconds = ref(0)
let minutes = ref(0)
let hours = ref(0)

function start() {
  if (!isRunning.value) {
    isRunning.value = true
    intervalId = setInterval(() => {
      seconds.value++
      if (seconds.value === 60) {
        seconds.value = 0
        minutes.value++
      }
      if (minutes.value === 60) {
        minutes.value = 0
        hours.value++
      }
    }, 1000)
  }
}

function stop() {
  if (isRunning.value) {
    clearInterval(intervalId)
    isRunning.value = false
  }
}

function reset() {
  stop()
  seconds.value = 0
  minutes.value = 0
  hours.value = 0
}

function formatTime() {
  const sec = seconds.value < 10 ? `0${seconds.value}` : seconds.value
  const min = minutes.value < 10 ? `0${minutes.value}` : minutes.value
  const hrs = hours.value < 10 ? `0${hours.value}` : hours.value
  return `${hrs}:${min}:${sec}`
}

onMounted(() => {
  start()
})
</script>

<style scoped>
.timer {
  width: 225px;
  background: #696969;
}
.timer:hover span {
  color: #fff;
}
.timer:hover .timer__item {
  border-bottom: 1px solid #fff;
}
.timer:hover .icon {
  filter: invert(1) grayscale(100%) brightness(10000%);
}
.timer:current span {
  color: #fff;
}
.timer:current .timer__item {
  border-bottom: 1px solid #fff;
}
.timer:current .icon {
  filter: invert(1) grayscale(100%) brightness(10000%);
}
.timer > div {
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.timer .timer__item {
  border-bottom: 1px solid #9E9E9E;
}
span {
  color: #9E9E9E;
}
.timer .timer__actions {
  display: flex;
  gap: 48px;
}
img {
  cursor: pointer;
}
</style>
