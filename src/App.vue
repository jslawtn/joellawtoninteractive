<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<template>
  <div id="app">
    <div class="container">
      <!-- <TicTacToe></TicTacToe> -->
      <MineSweeper></MineSweeper>
    </div>
  </div>
</template>

<script>
import TicTacToe from './components/TicTacToe.vue';
import MineSweeper from './components/MineSweeper.vue';
export default {
  name: 'app',
  components: {
    TicTacToe,
    MineSweeper
  }
}
</script>