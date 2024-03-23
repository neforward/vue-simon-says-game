<script setup>
import { ref } from "vue";

const isActive = ref(-1);
const combination = ref("");
const accumulator = ref("");
const round = ref(0);
const difficulty = ref("Easy");
const delayMs = ref(1.5);

const handleContinue = async (num) => {
  accumulator.value += num;
  const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

  if (combination.value.includes(accumulator.value)) {
    if (combination.value.length === accumulator.value.length) {
      round.value++;
      accumulator.value = "";
      combination.value += Math.floor(Math.random() * 4);
      for (let i = 0; i < combination.value.length; i++) {
        await delay((delayMs.value / 2) * 1000);
        isActive.value = combination.value[i];
        await delay((delayMs.value / 2) * 1000);
        isActive.value = -1;
      }
    }
  } else {
    combination.value = "";
    accumulator.value = "";
    round.value = 0;
  }
};

const changeDifficulty = (str, sec) => {
  difficulty.value = str;
  delayMs.value = sec;
};

const start = async () => {
  combination.value = "";
  accumulator.value = "";
  round.value = 0;
  combination.value += Math.floor(Math.random() * 4);

  const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

  for (let i = 0; i < combination.value.length; i++) {
    await delay((delayMs.value / 2) * 1000);
    isActive.value = combination.value[i];

    await delay((delayMs.value / 2) * 1000);
    isActive.value = -1;
  }
};
</script>

<template>
  <div>
    <h1>Simon says</h1>
    <div class="main">
      <div class="playground">
        <div
          @click="handleContinue(0)"
          class="blue"
          :class="+isActive === 0 ? 'active' : ''"
        ></div>
        <div
          @click="handleContinue(1)"
          class="red"
          :class="+isActive === 1 ? 'active' : ''"
        ></div>
        <div
          @click="handleContinue(2)"
          class="black"
          :class="+isActive === 2 ? 'active' : ''"
        ></div>
        <div
          @click="handleContinue(3)"
          class="green"
          :class="+isActive === 3 ? 'active' : ''"
        ></div>
      </div>
      <div class="options">
        <h2>Round: {{ round }}</h2>
        <p>Difficulty: {{ difficulty }}</p>
        <button @click="start">Start</button>
        <ul>
          <li>
            <button @click="changeDifficulty('Easy', 1.5)">Easy</button>
          </li>
          <li>
            <button @click="changeDifficulty('Normal', 1)">Normal</button>
          </li>
          <li>
            <button @click="changeDifficulty('Hard', 0.4)">Hard</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
#app {
  font-family: sans-serif;
}

h1 {
  text-align: center;
}

.playground {
  display: flex;
  width: 200px;
  height: 200px;
  flex-wrap: wrap;
}

.options {
  max-width: 200px;
}

.main {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 30px;
}

.playground div {
  height: 100px;
  width: 100px;
  opacity: 0.2;
}

.playground div:active {
  opacity: 1;
}

.red {
  background: red;
}

.blue {
  background: blue;
}

.black {
  background: #000;
}

.green {
  background: green;
}

.blue.active,
.green.active,
.red.active,
.black.active {
  opacity: 1;
}
</style>
