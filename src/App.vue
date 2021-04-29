<template>
  <div id="app">
    <Cube />
    <div style="margin-top:15px;">
     The github link is here <a href="https://github.com/reiddk/sudoku">https://github.com/reiddk/sudoku</a>
    </div>
  </div>
</template>

<script>
import Cube from './components/cube.vue'

export default {
  name: 'App',
  components: {
    Cube
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
