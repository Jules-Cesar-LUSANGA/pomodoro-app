<script setup>

import { computed, ref } from "vue";

const breakTime = ref(5)
const minutes = ref(25)
const seconds = ref(0)
const started = ref(false)

const stepText = ref('Focus')

// We will set our pomodoro setInterval here
const pomodoro = ref(null)

const minutesBySession = ref(null)

const formatedMinutes = computed(() => {
  return minutes.value >= 10 ? minutes.value : '0' + minutes.value;
})

const formatedSeconds = computed(() => {
  return seconds.value >= 10 ? seconds.value : '0' + seconds.value;
})


function start() {

  // Check if user typed negative or null minutes
  if (minutes.value < 0) {
    minutes.value = minutes.value * -1
  } else if (minutes.value == 0) {
    minutes.value = 25;
  }

  // Check if user typed negative or null break time
  if (breakTime.value < 0) {
    breakTime.value = breakTime.value * -1
  } else if (breakTime.value == 0) {
    breakTime.value = 5;
  }

  // Keep minutes by session
  minutesBySession.value = minutes.value

  started.value = true;
  seconds.value = 59
  minutes.value = minutes.value - 1

  // Begin our pomodoro timer
  pomodoro.value = setInterval(() => {

    seconds.value -= 1

    if (seconds.value == 0 && minutes.value > 0) {
      minutes.value -= 1
      seconds.value = 59
    }

    if (minutes.value == 0 && seconds.value == 0) {

      // Stop this session
      stop()

      // Take a break
      started.value = true;
      stepText.value = 'Break'

      // After Break time, restart session
      setTimeout(() => {
        
        minutes.value = minutesBySession.value
        stepText.value = 'Focus'
        start()
      }, 60000 * breakTime.value);
    }
  }, 1000);
}

function stop() {
  minutes.value = 25
  seconds.value = 0
  clearInterval(pomodoro.value)
  started.value = false;
}

</script>

<template>
  <div class="flex items-center justify-center h-screen font-mono">
    <div id="app-container">
      
      <h1 class="mb-3 font-bold text-2xl text-center text-violet-600">Pomodoro APP</h1>

      <div class="bg-gradient-to-tr from-violet-400 to-green-400 p-3 rounded-lg shadow-lg shadow-pink-300 flex justify-center">
        
        <div>
          <div class="inline-block border-8 rounded-full p-4 my-3">
            <div>
              <h2 class="font-bold text-6xl text-center">
                <span>{{ formatedMinutes }}</span>
                <span>:</span>
                <span>{{ formatedSeconds }}</span>
              </h2>
            </div>
            <h2 class="font-bold text-center">{{ stepText }}</h2>
          </div>

          <div class="flex justify-between mb-3" v-if="started == false">
            <div>
              <label for="time" class="font-bold block text-center">Time</label>
              <input type="number" name="time" id="time" class="w-20 px-2 text-center" min="25" value="25" v-model="minutes">
            </div>
            <div>
              <label for="break" class="font-bold block text-center">Break</label>
              <input type="number" name="break" id="break" class="w-20 px-2 text-center" min="5" value="5" v-model="breakTime">
            </div>
          </div>

          <div class="flex justify-center">
            <button class="bg-violet-800 hover:bg-gradient-to-tr hover:from-teal-500 hover:to-pink-600 text-white px-2 py-1 rounded" @click="start()" type="button" v-if="started == false">Start</button>
            <button class="bg-violet-800 hover:bg-gradient-to-tr hover:from-red-600 hover:to-yellow-900 text-white px-2 py-1 rounded" @click="stop()" type="button" v-if="started == true">Stop</button>
          </div>
          
        </div>
      </div>

      <p class="text-center mt-3">
        Made with <span class="text-red-500 text-2xl">&hearts;</span> By <a href="https://ljcesar.vercel.app" class="font-bold hover:text-blue-500 underline hover:no-underline">Jules-César LUSANGA</a>
      </p>
    </div>
    
  </div>
  
</template>