<template>
  <div class="hello">
<div class="scene">
  <div class="cube" v-bind:class="[ viewableSide ]">
    <div class="cube__face cube__face--front" >
      <Puzzle side="front" :puzzle="frontPuzzle" />
    </div>
    <div class="cube__face cube__face--back">
      <Puzzle side="back" :puzzle="backPuzzle" /></div>
    <div class="cube__face cube__face--right">
      <Puzzle side="right" :puzzle="rightPuzzle" /></div>
    <div class="cube__face cube__face--left">
      <Puzzle side="left" :puzzle="leftPuzzle" /></div>
    <div class="cube__face cube__face--top">
      <Puzzle side="top" :puzzle="topPuzzle" /></div>
    <div class="cube__face cube__face--bottom">
      <Puzzle side="bottom" :puzzle="bottomPuzzle" /></div>
  </div>
</div>
<p class="radio-group">
  <label>
    <input type="radio" name="rotate-cube-side" value="front" v-on:click="setSide('front')" /> front
  </label>
  <label>
    <input type="radio" name="rotate-cube-side" value="right" v-on:click="setSide('right')"  /> right
  </label>
  <label>
    <input type="radio" name="rotate-cube-side" value="back" v-on:click="setSide('back')" /> back
  </label>
  <label>
    <input type="radio" name="rotate-cube-side" value="left" v-on:click="setSide('left')" /> left
  </label>
  <label>
    <input type="radio" name="rotate-cube-side" value="top" v-on:click="setSide('top')" /> top
  </label>
  <label>
    <input type="radio" name="rotate-cube-side" value="bottom" v-on:click="setSide('bottom')" /> bottom
  </label>
</p>
<div>
  <select v-model="selectedPuzzle" @change="setPuzzles()">
    <option value="0">Puzzle 1</option>
    <option value="1">Puzzle 2</option>
    <option value="2">Puzzle 3</option>
  </select>
</div>

  </div>
</template>

<script>
import Puzzle from './puzzle.vue'
import sudoku from '../services/sudoku';

export default {
  name: 'Cube',
  components: {
    Puzzle
  },
  props: {
    msg: String
  },
  data: function () {
    return {
      viewableSide:'side-right',
      frontPuzzle: {},
      backPuzzle: {},
      rightPuzzle: {},
      leftPuzzle: {},
      topPuzzle: {},
      bottomPuzzle: {},
      selectedPuzzle: 0
    }
  },
  methods: {
    setSide(side) {
      this.viewableSide = `show-${side} side-${side}`;
    },
    setPuzzles() {
      if (isNaN(this.selectedPuzzle)) {
        this.selectedPuzzle = 0;
      }
      this.selectedPuzzle = Number(this.selectedPuzzle);
      this.frontPuzzle = sudoku.generateSudokuArr('front',this.selectedPuzzle);
      this.backPuzzle = sudoku.generateSudokuArr('back',this.selectedPuzzle);
      this.rightPuzzle = sudoku.generateSudokuArr('right',this.selectedPuzzle);
      this.leftPuzzle = sudoku.generateSudokuArr('left',this.selectedPuzzle);
      this.topPuzzle = sudoku.generateSudokuArr('top',this.selectedPuzzle);
      this.bottomPuzzle = sudoku.generateSudokuArr('bottom',this.selectedPuzzle);
      localStorage.setItem('lastChosenPuzzle', String(this.selectedPuzzle))
    }
  },
    mounted() {
      const lastChosenPuzzle = localStorage.getItem('lastChosenPuzzle') || 0;
      this.selectedPuzzle = lastChosenPuzzle;
      this.setPuzzles();
    }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* { box-sizing: border-box; }

body { font-family: sans-serif; }

.scene {
  width: 400px;
  height: 400px;
  margin: auto;
  perspective: 800px;

}

.cube {
  width: 400px;
  height: 400px;
  position: relative;
  transform-style: preserve-3d;
  transform: translateZ(-200px);
  transition: transform 1s;
}

.cube.show-front  { transform: translateZ(-200px) rotateY(   0deg); }
.cube.show-right  { transform: translateZ(-200px) rotateY( -90deg); }
.cube.show-back   { transform: translateZ(-200px) rotateY(-180deg); }
.cube.show-left   { transform: translateZ(-200px) rotateY(  90deg); }
.cube.show-top    { transform: translateZ(-200px) rotateX( -90deg); }
.cube.show-bottom { transform: translateZ(-200px) rotateX(  90deg); }

.cube__face {
  position: absolute;
  width: 400px;
  height: 400px;
  border: 2px solid black;
  font-size: 14px;
  font-weight: bold;
  color: black;
  text-align: center;
  background-color:white;
}



.cube__face--front  { transform: rotateY(  0deg) translateZ(200px); }
.cube__face--right  { transform: rotateY( 90deg) translateZ(200px); }
.cube__face--back   { transform: rotateY(180deg) translateZ(200px); }
.cube__face--left   { transform: rotateY(-90deg) translateZ(200px); }
.cube__face--top    { transform: rotateX( 90deg) translateZ(200px); }
.cube__face--bottom { transform: rotateX(-90deg) translateZ(200px); }

label { margin-right: 10px; }


.arrow {
  border: solid black;
  border-width: 0 3px 3px 0;
  display: inline-block;
  padding: 3px;
}

.right {
  transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
}

.left {
  transform: rotate(135deg);
  -webkit-transform: rotate(135deg);
}

.up {
  transform: rotate(-135deg);
  -webkit-transform: rotate(-135deg);
}

.down {
  transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
}


</style>
