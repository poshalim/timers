<template>
  <div class="timer-list">
    <my-timer v-for="n in numberOfTimers"></my-timer>
    <button class="timer__card timer__btn-add" @click="addTimer">
      <div></div>
      <div></div>
    </button>
  </div>
</template>

<script>
import MyTimer from "@/components/Timer";

import { ref } from "vue";

export default {
  name: "App",
  components: {
    MyTimer,
  },
  setup() {
    const numberOfTimers = ref(4); // Кол-во таймеров

    // Добавление нового таймера
    const addTimer = () => {
      numberOfTimers.value++;
    };
    return {
      numberOfTimers,
      addTimer,
    };
  },
};
</script>

<style lang="scss">
*,
*::after,
*::before {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body {
  background-color: #353638;
}

button {
  border: none;
  background-color: transparent;
  font-family: "Montserrat";
}

.timer-list {
  display: flex;
  flex-wrap: wrap;
  gap: 45px 50px;
  width: 805px;
  margin: 72px auto;
  padding: 0 15px;
}

.timer__btn-add {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;

  @media (any-hover: hover) {
    &:hover {
      opacity: 0.75;
    }
  }

  & div {
    width: 20px;
    height: 3px;
    background-color: #9e9e9e;
    position: absolute;
  }

  & div:first-child {
    transform: rotate(90deg);
  }
}

@media (max-width: 1024px) {
  .timer-list {
    width: 530px;
  }
}

@media (max-width: 768px) {
  .timer-list {
    width: 255px;
  }
}
</style>
