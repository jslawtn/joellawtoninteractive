<template>
  <div id="app">
    <checklist></checklist>
  </div>
</template>

<script>
import checklist from './components/CheckList.vue';
export default {
  name: 'app',
  components: {
    checklist
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
