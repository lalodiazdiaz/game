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

const winPercentage = () => {
  const total = wins.value + draws.value + losses.value;
  return total ? (wins.value / total) * 100 : 0;
};
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
    veredict.value = "It´s a draw";
  }

  SaveGame();
};

const SaveGame = () => {
  localStorage.setItem("wins", wins.value);
  localStorage.setItem("losses", losses.value);
  localStorage.setItem("draws", draws.value);
};

const loadGame = () => {
  wins.value = localStorage.getItem("wins");
  losses.value = localStorage.getItem("losses");
  draws.value = localStorage.getItem("draws");
};

const resetRound = () => {
  choice.value = null;
  computerChoise.value = null;
  veredict.value = null;
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
  <h1>Hello</h1>
</template>

<style></style>
