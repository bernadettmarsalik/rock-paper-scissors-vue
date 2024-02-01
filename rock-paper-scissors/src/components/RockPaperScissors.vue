<script setup lang="ts">
import { ref, watch } from "vue";
const playerChoice = ref<string>("");
const computerChoice = ref<string>("");
const result = ref<string>("");
const currentRound = ref<number>(0);
const playerPoints = ref<number>(0);
const computerPoints = ref<number>(0);
const roundWinner = ref<string>("");
const winner = ref<string>("");

function onclickRock(): void {
  playerChoice.value = "rock";
  computerChoice.value = computerSelector();
  gameRules(playerChoice.value, computerChoice.value);
}

function onclickPaper(): void {
  playerChoice.value = "paper";
  computerChoice.value = computerSelector();
  gameRules(playerChoice.value, computerChoice.value);
}

function onclickScissors(): void {
  playerChoice.value = "scissors";
  computerChoice.value = computerSelector();
  gameRules(playerChoice.value, computerChoice.value);
}

function computerSelector() {
  let computerPick = Math.round(Math.random() * (3 - 1) + 1);
  if (computerPick == 1) {
    return "paper";
  } else if (computerPick == 2) {
    return "rock";
  } else if (computerPick == 3) {
    return "scissors";
  } else {
    return "";
  }
}

function gameRules(playerChoice: string, computerChoice: string) {
  currentRound.value++;
  if (playerChoice == computerChoice) {
    return (result.value = "draw");
  } else if (
    (playerChoice == "paper" && computerChoice == "rock") ||
    (playerChoice == "rock" && computerChoice == "scissors") ||
    (playerChoice == "scissors" && computerChoice == "paper")
  ) {
    playerPoints.value++;
    return (result.value = "player");
  } else {
    computerPoints.value++;
    return (result.value = "computer");
  }
}

watch([result], () => {
  if (result.value != "") {
    if (result.value == "draw") {
      roundWinner.value = "It's draw";
    } else if (result.value == "player") {
      roundWinner.value = "You won!";
    } else roundWinner.value = "Computer won!";
  }
});
// Ide azért kell a !='' mert különben minden új kör elején a resetGame() után is kiírta az else ágat!

function whoWins() {
  if (playerPoints.value == computerPoints.value) {
    winner.value = "It's draw";
  } else if (playerPoints.value > computerPoints.value) {
    winner.value = "You won!";
  } else winner.value = "Computer won!";
}

watch([currentRound], () => {
  if (currentRound.value == 5) {
    whoWins();
  }
});

// Resetnél nullázzuk az összes eddigi értéket
function resetGame() {
  result.value = "";
  winner.value = "";
  playerPoints.value = 0;
  computerPoints.value = 0;
  currentRound.value = 0;
  roundWinner.value = "";
  playerChoice.value = "";
  computerChoice.value = "";
}

function surrender() {
  winner.value = "Computer won!";
  currentRound.value = 5;
}
</script>

<template>
  <div class="container p-5 rounded-5 shadow">
    <div id="game-body" class="p-3 rounded-5 shadow">
      <div class="container-body">
        <!-- -----------------------------EREDMÉNY RÉSZ----------------------------- -->

        <section class="text-center" v-if="winner">
          <h6 class="card-subtitle my-3 text-center text-danger fw-bold">
            *****GAME OVER!*****
          </h6>
          <ul class="list-group list-group-flush mt-3">
            <li
              :class="winner === 'You won!' ? 'bg-success' : 'bg-danger'"
              class="list-group-item text-center text-white fw-bold fs-3"
            >
              {{ winner }}
            </li>
          </ul>

          <button @click="resetGame()" class="btn btn-secondary w-50 mt-3">
            New Game
          </button>
        </section>

        <section v-else>
          <h6 class="card-subtitle my-3 text-body-secondary text-center">
            Choose one:
          </h6>
          <div class="d-flex justify-content-evenly">
            <button @click="onclickRock()" class="btn btn-secondary w-30">
              Rock
            </button>
            <button @click="onclickPaper()" class="btn btn-secondary w-30">
              Paper
            </button>
            <button @click="onclickScissors()" class="btn btn-secondary w-30">
              Scissors
            </button>
          </div>

          <hr class="opacity-25" />
          <h6 class="card-subtitle my-3 text-center text-success">
            You picked: {{ playerChoice }}
          </h6>
          <h6 class="card-subtitle my-3 text-center text-danger">
            Computer picked: {{ computerChoice }}
          </h6>
        </section>

        <!-- ----------------------------JÁTÉK RÉSZ----------------------------- -->

        <hr class="opacity-25" />

        <h6 class="card-subtitle my-3 text-body-secondary text-center">
          Player: {{ playerPoints }} - Computer: {{ computerPoints }}
        </h6>
        <h6 class="card-subtitle my-3 text-body-secondary text-center">
          Rounds left: {{ 5 - currentRound }}
        </h6>
        <ul class="list-group list-group-flush">
          <li v-if="!winner" class="list-group-item text-center bg-warning">
            {{ roundWinner }}
          </li>
        </ul>

        <div class="d-flex justify-content-center mt-4">
          <button
            v-if="!winner"
            @click="surrender()"
            class="btn btn-outline-secondary"
          >
            Surrender
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
