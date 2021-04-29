<template>
  <div id="app" v-bind:style="bgc">
    <nav v-bind:style="navColor">
      <div class="dark-light-mode-wrapper">
      <label class="switch">
        <input type="checkbox" v-model="darkMode" @change="changeDarkOrLightMode()">
        <span class="slider round"></span>
      </label>
      </div>
        <div class="light-dark-mode-label"> {{(darkMode)?'Dark': 'Light'}} Mode</div>
      <h1>Six Sided Sudoku</h1>
      <div class="audio-wrapper">
        <audio src="/assets/music.flac" autoplay controls>
           <p>If you are reading this, it is because your browser does not support the audio element.     </p>
        </audio>
      </div>
      </nav>
      <div style="margin-top:100px;">
    <Cube :darkMode="darkMode" />
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
  },  
  data: function () {
    return {
        darkMode: false,
        bgc: {
          backgroundColor: '',
          color:''
        },
        navColor: {
          backgroundColor: ''
        }
    }
  },
  methods: {
    changeDarkOrLightMode () {
      if (this.darkMode) {
        this.bgc.backgroundColor = 'black';
        this.bgc.color = 'white';
        this.navColor.backgroundColor = '#242424';
      } else {
        this.bgc.backgroundColor = '';
        this.bgc.color = '';
        this.navColor.backgroundColor = '';
      }
      localStorage.setItem('darkMode', this.darkMode);
    }
  },
  mounted() {
    this.darkMode = localStorage.getItem('darkMode') === 'true' || false;
    this.changeDarkOrLightMode();
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
  position:absolute;
  top:0px;
  bottom:0px;
  left:0px;
  right:0px;
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
.dark-light-mode-wrapper {
  position: absolute;
    left: 15px;
    top: 22px;
}

.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

.switch input { 
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}

.light-dark-mode-label {
    font-weight: bold;
    display: inline-block;
    position: absolute;
    left: 80px;
    top: 30px;
}

@media only screen and (max-width: 440px) {
.light-dark-mode-label {
    display:none;
}
}
</style>
