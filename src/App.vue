<script setup>
import { ref, computed, onMounted } from "vue";

const wins = ref(0);
const draws = ref(0);
const losses = ref(0);

const choice = ref(null);
const computerChoise = ref(null);
const veredict = ref(null);

const outcomes = {
  rock: {
    rock: "draw",
    paper: "loss",
    scissors: "win",
  },
  paper: {
    rock: "win",
    paper: "draw",
    scissors: "loss",
  },
  scissors: {
    rock: "loss",
    paper: "win",
    scissors: "draw",
  },
};

const winPercentage = computed(() => {
  const total = wins.value + draws.value + losses.value;
  console.log(total);
  return total ? (wins.value / total) * 100 : 0;
});
const play = (c) => {
  choice.value = c;

  const choises = ["rock", "paper", "scissors"];
  const random = Math.floor(Math.random() * choises.length);
  computerChoise.value = choises[random];

  const outcome = outcomes[c][computerChoise.value];

  if (outcome === "win") {
    wins.value++;
    veredict.value = "You win!!";
  } else if (outcome == "loss") {
    losses.value++;
    veredict.value = "You lose!!";
  } else {
    draws.value++;
    veredict.value = "It\´s a draw";
  }

  SaveGame();
};

const SaveGame = () => {
  localStorage.setItem("wins", wins.value);
  localStorage.setItem("losses", losses.value);
  localStorage.setItem("draws", draws.value);
};

const loadGame = () => {
  wins.value = parseInt(localStorage.getItem("wins")) || 0;
  losses.value = parseInt(localStorage.getItem("losses")) || 0;
  draws.value = parseInt(localStorage.getItem("draws")) || 0;
};

const resetRound = () => {
  choice.value = null;
  computerChoise.value = null;
  veredict.value = null;
};

const deleteScore = () => {
  localStorage.removeItem("wins");
  localStorage.removeItem("losses");
  localStorage.removeItem("draws");
  wins.value = 0;
  draws.value = 0;
  losses.value = 0;
  resetRound();
};

onMounted(() => {
  loadGame();
  window.addEventListener("keypress", (e) => {
    if (e.key === "r") {
      resetRound();
    }
  });
});
</script>

<template>
  <div class="bg-gray-700 text-white text-center min-h-screen flex flex-col">
    <header class="container mx-auto p-6">
      <h1 class="text-4xl font-bold">Rock, Paper, Scissors</h1>
    </header>
    <main class="container mx-auto p-6 flex-1">
      <div
        v-if="choice === null"
        class="flex items-center justify-center mx-6 max-[1000px]:flex-col"
      >
        <button
          @click="play('rock')"
          class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-pink-500 max-[1000px]:w-52 mb-2"
        >
          <img src="./assets/RockIcon.svg" alt="Rock" class="w-full" />
        </button>
        <button
          @click="play('paper')"
          class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-green-500 max-[1000px]:w-52 mb-2"
        >
          <img src="./assets/PaperIcon.svg" alt="Rock" class="w-full" />
        </button>
        <button
          @click="play('scissors')"
          class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-yellow-500 max-[1000px]:w-52 mb-2"
        >
          <img src="./assets/ScissorsIcon.svg" alt="Rock" class="w-full" />
        </button>
      </div>
      <div v-else >
        <div class="text-3xl mb-4">
          You picket <span class="text-pink-500"> {{ choice }}</span>
        </div>
        <div class="text-3xl mb-4">
          You picket <span class="text-green-500"> {{ computerChoise }}</span>
        </div>
        <div class="text-6xl mb-12 animate-pulse">{{ veredict }}</div>
        <div class="flex w-full justify-around">
          <button
            @click="resetRound"
            class="bg-pink-500 text-lg py-2 px-4 w-32 rounded-full h-32 transition-colors duration-500 hover:bg-yellow-50 hover:text-black"
          >
            Again
          </button>
          <button
            @click="deleteScore"
            class="bg-pink-500 text-lg py-2 px-4 w-32 rounded-full h-32 transition-colors duration-500 hover:bg-yellow-50 hover:text-black"
          >
            Reset
          </button>
        </div>
      </div>
      <div class="mt-12 text-3xl mb-4">
        {{ wins }} : {{ draws }} : {{ losses }}
      </div>
      <div class="text-lg">Win rate: {{ Math.round(winPercentage) }}%</div>
    </main>
  </div>
</template>

<style></style>
