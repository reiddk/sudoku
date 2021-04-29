<template>
  <div class="hello">
<div class="scene">
  <div class="cube" v-bind:class="[ viewableSideClass ]">
    <div class="cube__face cube__face--front" >
      <Puzzle side="front" :viewingside="theSide" :puzzle="frontPuzzle" :puzzleNum="selectedPuzzle"/>
    </div>
    <div class="cube__face cube__face--back">
      <Puzzle side="back" :viewingside="theSide" :puzzle="backPuzzle" :puzzleNum="selectedPuzzle" /></div>
    <div class="cube__face cube__face--right">
      <Puzzle side="right" :viewingside="theSide"  :puzzle="rightPuzzle"  :puzzleNum="selectedPuzzle" /></div>
    <div class="cube__face cube__face--left">
      <Puzzle side="left" :viewingside="theSide"  :puzzle="leftPuzzle"  :puzzleNum="selectedPuzzle" /></div>
    <div class="cube__face cube__face--top">
      <Puzzle side="top" :viewingside="theSide"  :puzzle="topPuzzle"  :puzzleNum="selectedPuzzle" /></div>
    <div class="cube__face cube__face--bottom">
      <Puzzle side="bottom" :viewingside="theSide"  :puzzle="bottomPuzzle"  :puzzleNum="selectedPuzzle" /></div>
  </div>
</div>
<p class="radio-group" style="margin-top:100px;">
  <label>
    <input type="radio"  v-model="theSide" name="rotate-cube-side" value="front" v-on:click="setSide('front')" /> Front
  </label>
  <label>
    <input type="radio" v-model="theSide"  name="rotate-cube-side" value="right" v-on:click="setSide('right')"  /> Right
  </label>
  <label>
    <input type="radio" v-model="theSide"  name="rotate-cube-side" value="back" v-on:click="setSide('back')" /> Back
  </label>
  <label>
    <input type="radio" v-model="theSide"  name="rotate-cube-side" value="left" v-on:click="setSide('left')" /> Left
  </label>
  <label>
    <input type="radio" v-model="theSide"  name="rotate-cube-side" value="top" v-on:click="setSide('top')" /> Top
  </label>
  <label>
    <input type="radio" v-model="theSide"  name="rotate-cube-side" value="bottom" v-on:click="setSide('bottom')" /> Bottom
  </label>
</p>
<div>
  <select v-model="selectedPuzzle" @change="setPuzzles()">
    <option value="0">Difficulty 1</option>
    <option value="1">Difficulty 2</option>
    <option value="2">Difficulty 3</option>
    <option value="3">Difficulty 4</option>
    <option value="4">Difficulty 5</option>
    <option value="5">Difficulty 6</option>
    <option value="6">Difficulty 7</option>
    <option value="7">Difficulty 8</option>
    <option value="8">Difficulty 9</option>
    <option value="9">Difficulty 10</option>
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
      viewableSideClass:'',
      theSide: '',
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
      this.theSide = side;
      this.viewableSideClass = `show-${side} side-${side}`;
      localStorage.setItem('showingside', String(side));
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
      localStorage.setItem('lastChosenPuzzle', String(this.selectedPuzzle));
    }
  },
    mounted() {
      const lastChosenPuzzle = localStorage.getItem('lastChosenPuzzle') || 0;
      this.selectedPuzzle = lastChosenPuzzle;
      this.setPuzzles();

      this.setSide(localStorage.getItem('showingside') || `front`);

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
  margin:auto;
  transform-style: preserve-3d;
  transform: translateZ(-200px);
  transition: transform 1s;
  margin-top:25px;
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

@media only screen and (max-width: 440px) {
 .scene {
  width: 200px;
  height: 200px;
  margin: auto;
  perspective: 400px;

}

.cube {
  width: 200px;
  height: 200px;
  position: relative;
  margin:auto;
  transform-style: preserve-3d;
  transform: translateZ(-100px);
  transition: transform 1s;
  margin-top:25px;
}

.cube.show-front  { transform: translateZ(-100px) rotateY(   0deg); }
.cube.show-right  { transform: translateZ(-100px) rotateY( -90deg); }
.cube.show-back   { transform: translateZ(-100px) rotateY(-180deg); }
.cube.show-left   { transform: translateZ(-100px) rotateY(  90deg); }
.cube.show-top    { transform: translateZ(-100px) rotateX( -90deg); }
.cube.show-bottom { transform: translateZ(-100px) rotateX(  90deg); }

.cube__face {
  position: absolute;
  width: 200px;
  height: 200px;
  border: 2px solid black;
  font-size: 10px;
  font-weight: bold;
  color: black;
  text-align: center;
  background-color:white;
}


.cube__face--front  { transform: rotateY(  0deg) translateZ(100px); }
.cube__face--right  { transform: rotateY( 90deg) translateZ(100px); }
.cube__face--back   { transform: rotateY(180deg) translateZ(100px); }
.cube__face--left   { transform: rotateY(-90deg) translateZ(100px); }
.cube__face--top    { transform: rotateX( 90deg) translateZ(100px); }
.cube__face--bottom { transform: rotateX(-90deg) translateZ(100px); } 

}


label { margin-right: 10px; }


.arrow {
border: solid black;
    border-width: 0 6px 6px 0;
    display: inline-block;
    padding: 10px;
    position: absolute;
    cursor: pointer;
}

.arrow:hover {
  opacity:.5;
}

.right {
  transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
  right:0px;
  top:47%;
}

.left {
  transform: rotate(135deg);
  -webkit-transform: rotate(135deg);
  left:0px;
  top:47%;
}

.up {
  transform: rotate(-135deg);
  -webkit-transform: rotate(-135deg);
  top:0px;
  left: 47%;
}

.down {
  transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
  bottom:0px;
  left: 47%;
}


</style>
