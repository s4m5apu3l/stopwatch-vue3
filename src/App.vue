<script setup>
import { ref, watch, onUnmounted, reactive } from "vue";

const stopwatches = reactive([
  {
    startTime: null,
    elapsedTime: 0,
    timer: null,
    isRunning: false,
  },
]);

const startStopwatch = (index) => {
  const stopwatch = stopwatches[index];
  stopwatch.startTime = Date.now() - stopwatch.elapsedTime;
  stopwatch.timer = setInterval(() => {
    stopwatch.elapsedTime = Date.now() - stopwatch.startTime;
  }, 1000);
  stopwatch.isRunning = true;
};

const stopStopwatch = (index) => {
  const stopwatch = stopwatches[index];
  clearInterval(stopwatch.timer);
  stopwatch.isRunning = false;
};

const resetStopwatch = (index) => {
  const stopwatch = stopwatches[index];
  clearInterval(stopwatch.timer);
  stopwatch.elapsedTime = 0;
  stopwatch.isRunning = false;
};

const formatTime = (time) => {
  const seconds = Math.floor(time / 1000);

  if (seconds < 60) {
    return `${seconds.toString().padStart(1, '0')}`;
  } else {
    const minutes = Math.floor(seconds / 60);
    const remainingSeconds = seconds % 60;
    return `${minutes.toString().padStart(2, '0')}:${remainingSeconds.toString().padStart(2, '0')}`;
  }
};

const addStopwatch = () => {
  stopwatches.push({
    startTime: null,
    elapsedTime: 0,
    timer: null,
    isRunning: false,
  });
};
</script>

<template>
  <div>
    <div v-for="(stopwatch, index) in stopwatches" :key="index">
      <p>Elapsed time: {{ formatTime(stopwatch.elapsedTime) }}</p>
      <button @click="startStopwatch(index)" v-if="!stopwatch.isRunning">
        Start
      </button>
      <button @click="stopStopwatch(index)" v-else>Stop</button>
      <button @click="resetStopwatch(index)">Reset</button>
    </div>
    <button @click="addStopwatch">+</button>
  </div>
</template>

<style scoped></style>
