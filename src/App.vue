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
      <!-- <h5>Check List</h5>
      <checklist></checklist> -->
      <grid></grid>
    </div>
  </div>
</template>

<script>
//import checklist from './components/CheckList.vue';
import grid from './components/Grid.vue';
export default {
  name: 'app',
  components: {
    //checklist,
    grid
  }
}
</script>