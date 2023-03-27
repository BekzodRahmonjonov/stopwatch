<template>
  <div class="container">
    <Timer
        v-for="(timer, index) in timers"
        :key="index"
        :time="timer.time"
        :is-running="timer.isRunning"
        @start-timer="startTimer(index)"
        @stop-timer="stopTimer(index)"
        @reset-timer="resetTimer(index)"
    />
    <div class="add-timer" @click="addTimer">
      <img src="./assets/add.svg" alt="Add timer">
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Timer from './components/Timer.vue'

const timers = ref([])

function addTimer() {
  timers.value.push({
    time: "00:00:00",
    isRunning: false,
    intervalId: null
  })
}

function startTimer(index) {
  const timer = timers.value[index]
  if (!timer.isRunning) {
    timer.isRunning = true
    timer.intervalId = setInterval(() => {
      const [hours, minutes, seconds] = timer.time.split(":").map(Number)
      let newSeconds = seconds + 1
      let newMinutes = minutes
      let newHours = hours
      if (newSeconds === 60) {
        newSeconds = 0
        newMinutes = minutes + 1
      }
      if (newMinutes === 60) {
        newMinutes = 0
        newHours = hours + 1
      }
      if (newHours === 24) {
        newHours = 0
      }
      timer.time = `${String(newHours).padStart(2, '0')}:${String(newMinutes).padStart(2, '0')}:${String(newSeconds).padStart(2, '0')}`
    }, 1000)
  }
}

function stopTimer(index) {
  const timer = timers.value[index]
  if (timer.isRunning) {
    clearInterval(timer.intervalId)
    timer.isRunning = false
  }
}

function resetTimer(index) {
  const timer = timers.value[index]
  clearInterval(timer.intervalId)
  timer.time = "00:00:00"
  timer.isRunning = false
}

</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background: rgb(53,54,56);
}
.container {
  padding: 50px;
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 50px;
  flex-wrap: wrap;
}
.add-timer {
  width: 225px;
  background: #696969;
  height: 120px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}
.add-timer:hover img {
  filter: invert(1) grayscale(100%) brightness(10000%);
}
@media screen and (max-width: 420px) {
  .container {
    padding: 50px 10px;
  }
}
</style>
