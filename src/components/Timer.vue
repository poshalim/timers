<template>
  <div :class="['timer', 'timer__card', { 'timer--active': timer }]">
    <div class="timer__elapsed-time">
      <span v-if="hours > 0">{{ hours }}:</span>
      <span v-if="minutes > 0 || hours > 0">{{ minutes }}:</span>
      {{ seconds }}
    </div>
    <div class="timer__btns">
      <button class="timer__btn" @click="toggleTimerWork">
        <div class="timer__btn-pause" v-if="timer">
          <div></div>
          <div></div>
        </div>

        <div class="timer__btn-start" v-else></div>
      </button>

      <button :class="['timer__btn timer__btn-reset', { disabled: !elapsedTime }]" @click="resetTimer"
        :disabled="!elapsedTime"></button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";

export default {
  name: "MyTimer",
  setup() {
    const startTime = ref(null)
    const elapsedTime = ref(0); // Пройденное время
    let timer = ref(false);

    const hours = computed(() => {
      let hours = Math.floor(elapsedTime.value / 1000 / 60 / 60);
      return hours;
    });

    const minutes = computed(() => {
      let minutes = Math.floor((elapsedTime.value / 1000 / 60) % 60);
      return minutes < 10 ? "0" + minutes : minutes;
    });

    const seconds = computed(() => {
      let seconds = Math.floor((elapsedTime.value / 1000) % 60);
      return seconds < 10 ? "0" + seconds : seconds;
    });

    // Запуск/остановка таймера
    const toggleTimerWork = () => {
      if (timer.value) {
        clearInterval(timer.value);
        timer.value = false;
      }
      else {
        startTime.value = new Date() - elapsedTime.value;

        timer.value = setInterval(() => {
          elapsedTime.value = new Date() - startTime.value;
        }, 100);
      }
    };

    // Сброс таймера
    const resetTimer = () => {
      elapsedTime.value = 0;
      clearInterval(timer.value);
      timer.value = false;
    };

    return {
      timer,
      hours,
      minutes,
      seconds,
      elapsedTime,
      toggleTimerWork,
      resetTimer,
    };
  },
};
</script>

<style lang="scss">
@import url("https://fonts.cdnfonts.com/css/gotham-pro?styles=24950");

.timer {
  &--active {

    .timer__elapsed-time,
    .timer__btns {
      color: #fff;
      border-color: #fff;
    }

    .timer__btn-start,
    .timer__btn-pause div,
    .timer__btn-reset {
      background-color: #fff;
    }
  }

  &__card {
    width: 225px;
    height: 120px;
    background-color: #696969;
  }

  &__elapsed-time {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50%;
    font-family: "Gotham Pro", sans-serif;
    font-size: 22px;
    line-height: 21px;
    text-align: center;
    color: #9e9e9e;
    border-bottom: 1px solid #9e9e9e;
  }

  &__btns {
    height: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 45px;
    border-top: 1px solid #9e9e9e;
  }

  &__btn {
    height: 20px;
    width: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: opacity 0.5s ease;

    @media (any-hover: hover) {
      &:hover:not([disabled]) {
        opacity: 0.75;
      }
    }

    &-start {
      width: 0;
      height: 0;
      border-top: 10px solid transparent;
      border-bottom: 10px solid transparent;
      border-left: 17px solid #9e9e9e;
    }

    &-pause {
      width: 100%;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 4px;

      & div {
        background-color: #9e9e9e;
        height: 100%;
        width: 3px;
      }
    }

    &-reset {
      background-color: #9e9e9e;
    }
  }
}

.disabled {
  opacity: 0.5;
}
</style>
