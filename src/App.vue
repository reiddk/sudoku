<template>
  <div id="app">
    <nav>
      <h1>Six Sided Sudoku</h1>
      <div class="audio-wrapper">
        <audio src="/assets/music.flac" autoplay controls>
<p>If you are reading this, it is because your browser does not support the audio element.     </p>
</audio>
      </div>
      </nav>
      <div style="margin-top:100px;">
    <Cube />
      </div>
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
nav {
  position:fixed;
  top:0px;
  left:0px;
  right:0px;
  background-color:rgb(222, 222, 222);
-webkit-box-shadow: 0px 2px 2px 1px rgba(0,0,0,0.39); 
box-shadow: 0px 2px 2px 1px rgba(0,0,0,0.39);
}
.audio-wrapper {
  position: absolute;
    right: 15px;
    top: 22px;
}
audio {
  width:55px;
  height:25px;
  transition: width .5s;
}
audio:hover {
  width:300px;
}
</style>
