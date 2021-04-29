<template>
<div class="sudoku-puzzle">
  <div class="sudoku-block" v-for="(item, index) in puzzle.game" :key="index">
      <div class="sudoku-block-content" :class="{'clickable': !puzzle.defaultPieces.includes(index), 'answer-incorrect': incorrectNumbers.includes(index) }" v-on:click="setSelectedSquae(index)">
          <div style="" class="block-text-wrapper" :class="{'hide-text': !item, 'opacity-text':puzzle.defaultPieces.includes(index) }">{{ (item)?item:1 }}</div>
      </div>
          <div class="possibilities" :class="{'show-possibilities': index === selectedSquare}">
              <div class="possiblility" v-for="(item, index) in possibilites" :key="index"  v-on:click="setSquare(item)">
                  {{item}}
                </div>
          </div>
  </div>
  <div class="thick-border-wrapper-width" style="top:0px;left:0px;">
      <div class="thick-border-bottom" ></div>
  </div>
  <div class="thick-border-wrapper-width" style="top:33.33%;left:0px;">
      <div class="thick-border-bottom" ></div>
  </div>
  <div class="thick-border-wrapper-height" style="top:0px;left:0px;">
      <div class="thick-border-right" ></div>
  </div>
  <div class="thick-border-wrapper-height" style="top:0px;left:33.33%;">
      <div class="thick-border-right" ></div>
  </div>
  <div class="deselect-square" :class="{'hideit': selectedSquare === null}" v-on:click="deselectSquare()"></div>
  <div style="margin-top:15px;" v-if="side === viewingside">
      <div style="float:left;">
        <button v-on:click="undo()" :disabled="puzzle.history.length === 0">Undo</button>&nbsp;
        <button v-on:click="redo()" :disabled="puzzle.redo.length === 0">Redo</button>
      </div>
      <div style="float:right;">
        <label v-bind:style="{ color: darkMode?'white':'black' }">Check answers<input type="checkbox" id="checkanswers" name="checkanswers" v-model="check" @change="checkAnswers()"></label>
            
      </div>
      </div>
</div>
</template>

<script>
export default {
  name: 'Puzzle',
  props: {
    side: String,
    puzzleNum: Number,
    puzzle: Object,
    viewingside: String,
    darkMode: Boolean
  },
  data: function () {
    return {
        selectedSquare: null,
        possibilites: [1,2,3,4,5,6,7,8,9],
        check: false,
        incorrectNumbers: []
    }
  },
  methods: {
    setSelectedSquae(squareIndex) {
        if (this.puzzle.defaultPieces.includes(squareIndex)) {
            this.deselectSquare();
            return;
        }
      this.selectedSquare = squareIndex;
    },
    deselectSquare() {
        this.selectedSquare = null;
    },
    setSquare(squareVal) {
        if (this.selectedSquare === null) {
            return;
        }
        this.puzzle.history.unshift([...this.puzzle.game]);
        this.puzzle.game[this.selectedSquare] = squareVal;
        this.selectedSquare = null;
        this.puzzle.redo = [];
        localStorage.setItem(`${this.side}_${this.puzzleNum}`, JSON.stringify(this.puzzle));
        this.checkAnswers();
    },
    undo() {
        if (!this.puzzle.history.length) {
            return;
        }
        const lastGame = this.puzzle.history.shift();
        this.puzzle.redo.unshift([...this.puzzle.game]);
        this.puzzle.game = lastGame;
        localStorage.setItem(`${this.side}_${this.puzzleNum}`, JSON.stringify(this.puzzle));
        this.checkAnswers();
    },
    redo() {
        if (!this.puzzle.redo.length) {
            return;
        }
        const lastGame = this.puzzle.redo.shift();
        this.puzzle.history.unshift([...this.puzzle.game]);
        this.puzzle.game = lastGame;
        localStorage.setItem(`${this.side}_${this.puzzleNum}`, JSON.stringify(this.puzzle));
        this.checkAnswers();
    },
    checkAnswers() {
        this.incorrectNumbers = [];
        if (!this.check) {
            return;
        }
        
        for (let i = 0; i < this.puzzle.completed.length; i++) {
            if (this.puzzle.game[i] !== null &&
                this.puzzle.game[i] !== this.puzzle.completed[i]) {
                    this.incorrectNumbers.push(i);
            }
        }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.sudoku-puzzle {
    display:inline-block;
    height:100%;
    position:relative;
}
.sudoku-block {
    width:11.11%;
    height:11.11%;
    display:inline-block;
    position:relative;
}
.sudoku-block-content {
    width:100%;
    height:100%;
    position:relative;
    z-index:10;
    border:solid black 1px;
}
.thick-border-wrapper-width {
    position:absolute;
    width:100%;
    height:33.33%;
}
.thick-border-wrapper-height {
    position:absolute;
    width:33.33%;
    height:100%;
}
.thick-border-bottom {
    border-bottom:solid black 4px;
    height: 100%;
}
.thick-border-right {
    border-right:solid black 4px;
    width: 100%;
    height:100%;
}
.block-text-wrapper {
margin-top:15px;
-webkit-user-select: none; /* Safari */        
-moz-user-select: none; /* Firefox */
-ms-user-select: none; /* IE10+/Edge */
user-select: none; /* Standard */
}
.hide-text {
    visibility:hidden;
}
.hideit {
    display:none;
}
.clickable {
    cursor:pointer;
}
.opacity-text {
    opacity:.5;
}
.possibilities {
    width: 0px;
    height: 0px;
    background-color: white;
    position: absolute;
    z-index: 12;
    top: 15px;
    left: 15px;
    transition: height .5s;
    overflow-y:hidden;
}
.show-possibilities {
    width: 100px;
    height: 242px;
    border: solid 1px black;
}
.deselect-square {
    opacity:.5;
    background-color:grey;
    top:0px;
    bottom:0px;
    left:0px;
    right:0px;
    position:fixed;
    z-index:11;
    cursor:pointer;
}
.possiblility {
    border-bottom:1px solid rgb(169, 169, 169);
    cursor:pointer;
    background-color:white;
    padding:5px;

}
.possiblility:hover {
   -webkit-box-shadow: inset 0px 0px 1px 1px #bfbfbf; 
box-shadow: inset 0px 0px 1px 1px #bfbfbf; 
}
.answer-incorrect {
    background-color:rgba(255, 0, 0, 0.297);
}

@media only screen and (max-width: 440px) {
.block-text-wrapper {
    margin-top: 5px;
}
}
</style>
