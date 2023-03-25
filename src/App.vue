<script setup>
import { ref, watch, onUnmounted, reactive } from "vue";

const stopwatches = reactive([
  {
    startTime: null,
    elapsedTime: 0,
    timer: null,
    isActive: false,
  },
]);

// const isActive = ref(false);

const startStopwatch = (index) => {
  const stopwatch = stopwatches[index];
  stopwatch.startTime = Date.now() - stopwatch.elapsedTime;
  stopwatch.timer = setInterval(() => {
    stopwatch.elapsedTime = Date.now() - stopwatch.startTime;
  }, 1000);
  stopwatch.isActive = true;
};

const stopStopwatch = (index) => {
  const stopwatch = stopwatches[index];
  clearInterval(stopwatch.timer);
  stopwatch.isActive = false;
};

const resetStopwatch = (index) => {
  const stopwatch = stopwatches[index];
  clearInterval(stopwatch.timer);
  stopwatch.elapsedTime = 0;
  stopwatch.isActive = false;
};

const formatTime = (time) => {
  const seconds = Math.floor(time / 1000);
  const minutes = Math.floor(seconds / 60);
  const hours = Math.floor(minutes / 60);
  const remainingMinutes = minutes % 60;
  const remainingSeconds = seconds % 60;

  if (hours > 0) {
    return `${hours.toString().padStart(2, "0")}:${remainingMinutes
      .toString()
      .padStart(2, "0")}:${remainingSeconds.toString().padStart(2, "0")}`;
  } else if (minutes > 0) {
    return `${remainingMinutes.toString().padStart(2, "0")}:${remainingSeconds
      .toString()
      .padStart(2, "0")}`;
  } else {
    return `${remainingSeconds.toString().padStart(1, "0")}`;
  }
};
const addStopwatch = () => {
  stopwatches.push({
    startTime: null,
    elapsedTime: 0,
    timer: null,
    isActive: false,
  });
};
</script>

<template>
  <section>
    <div class="l-wrapper">
      <div class="stopwatch">
        <div
          class="stopwatch-card"
          :class="{ active: stopwatch.isActive }"
          v-for="(stopwatch, index) in stopwatches"
          :key="index"
        >
          <div class="stopwatch-card__top">
            <span class="">{{ formatTime(stopwatch.elapsedTime) }}</span>
          </div>
          <div class="stopwatch-card__bottom">
            <div @click="startStopwatch(index)" v-if="!stopwatch.isActive">
              <svg
                width="17"
                height="20"
                viewBox="0 0 17 20"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path d="M0 20V0L17 10L0 20Z" />
              </svg>
            </div>
            <div @click="stopStopwatch(index)" v-else>
              <svg
                width="10"
                height="20"
                viewBox="0 0 10 20"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <rect x="7" width="3" height="20"  />
                <rect width="3" height="20" />
              </svg>
            </div>
            <div @click="resetStopwatch(index)">
              <svg
                width="20"
                height="20"
                viewBox="0 0 20 20"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <rect width="20" height="20" />
              </svg>
            </div>
          </div>
        </div>
        <div class="stopwatch-add" @click="addStopwatch"></div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.active {
  background-color:  #9E9E9E;
 
}


.active .stopwatch-card__top {
  border-bottom:  1px solid white;
  color: white;
}

.stopwatch svg {
  fill: #9E9E9E;
  width: 20px;
  cursor: pointer;

}

.active svg {
  fill: white !important;
}
</style>
