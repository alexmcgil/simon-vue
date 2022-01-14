<template>
  <div>
    <div class="game">
      <div class="simon">
        <ul v-on:click="action">
<!--        <ul v-on:click="blink">-->
          <li  class="tile red"></li>
          <li class="tile blue"></li>
          <li class="tile yellow"></li>
          <li class="tile green"></li>
        </ul>
      </div>
    </div>
    <div class="info">
      <button v-on:click="startGame">Играть!</button>
      <p>Вы проиграли :(</p>
    </div>
    <div class="difficulty">
      <h2>Сложность:</h2>
      <input type="radio" v-model="difficulty" name="mode" value="easy" id="easy" checked><label
        for="easy">Лёгкая</label><br>
      <input type="radio" v-model="difficulty" name="mode" value="normal" id="normal"><label
        for="normal">Средняя</label><br>
      <input type="radio" v-model="difficulty" name="mode" value="hard" id="hard"><label for="hard">Сложная</label>
    </div>
  </div>
</template>

<script>

let soundOne = new Audio(require("./../../sounds/1.mp3"))
let soundTwo = new Audio(require("./../../sounds/2.mp3"))
let soundThree = new Audio(require("./../../sounds/3.mp3"))
let soundFour = new Audio(require("./../../sounds/4.mp3"))


export default {
  name: 'Simon',
  data: () => {
    return {
      difficulty: "easy",
      playerSteps: [],
      simonsSteps: [],
      sequence: [],
      step: 0,
    }
  },
  methods: {

    getDifficulty(){
      switch (this.difficulty){
        case "easy":
          return 1500;
        case "normal":
          return 1000;
        case "hard":
          return 400;
      }
    },

    action(e) {
      if (this.isPlayer(e)){
        this.playSound(e.target.className)
        this.blink(e)
        this.playerSteps.push(e.target.className)
      } else {
        let pieces = document.querySelectorAll("li")
        console.log(e)
        switch (e) {
          case "tile red":
            this.blink(pieces[0]);
            this.playSound(e)
            break;

            case "tile blue":
            this.blink(pieces[1]);
              this.playSound(e)
            break;

            case "tile yellow":
            this.blink(pieces[2]);
              this.playSound(e)
            break;

            case "tile green":
            this.blink(pieces[3]);
              this.playSound(e)
            break;
        }
      }
    },

    getRandomTile() {
      switch (Math.floor(Math.random() * (4 - 1 + 1)) + 1) {
        case 1:
          return "tile red"

        case 2:
          return "tile blue"

        case 3:
          return "tile yellow"

        case 4:
          return "tile green"

      }

    },

    playSound(step) {
      switch (step) {
        case "tile red":
          soundOne.play()
          break;

        case "tile blue":
          soundTwo.play()
          break;

        case "tile yellow":
          soundThree.play()
          break;

        case "tile green":
          soundFour.play()
          break;

      }
    },

    blink(stepOrEvent) {
      let area
      if (this.isPlayer(stepOrEvent)) {
        area = stepOrEvent.target;
        console.log(area)
      }else{
        area = stepOrEvent
        console.log(area)
      }
      console.log(area)
      area.classList.add("blink")
      setTimeout(function() {
        return function() {
          area.classList.toggle("blink");
        };
      }(), 150);
    },

    isPlayer(e) {
      switch (e.target){
        case undefined:
          return false
        default:
          return true
      }
    },

        startGame() {
      // this.playerSteps = []
      // this.simonsSteps = []

      this.Simon()
    },
    Simon(){
      let that = this;
      let bup = that.getRandomTile()
      that.simonsSteps.push(bup)
      let steps = that.simonsSteps
      for(let currentIndex=0; currentIndex<that.simonsSteps.length; currentIndex++) {
        setTimeout(function() {
          return function() {
            that.action(bup)
          };
        }(currentIndex), that.getDifficulty()*(currentIndex));
      }
    }
  }
}
</script>

