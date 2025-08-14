<template>
  <div class="app">
    <h1>Vue 3 圈叉棋</h1>
    <GameBoard />
  </div>
</template>

<script setup>
import GameBoard from './components/GameBoard.vue'
</script>

<style>
.app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 50px;
}
</style>