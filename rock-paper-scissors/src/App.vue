<script setup lang="ts">
import "bootstrap/dist/css/bootstrap.min.css";
import RockPaperScissors from "./components/RockPaperScissors.vue";
</script>

<template>
  <div id="game" class="container-fluid vh-100 vw-100 pt-5">
    <header class="text-center">
      <h1>Rock Paper Scissors Game</h1>
    </header>
    <RockPaperScissors />
  </div>
</template>

<style scoped></style>
