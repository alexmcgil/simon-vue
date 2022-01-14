<template>
  <div class="wrapper">
    <div class="game">
      <div class="simon">
        <ul v-on:click="actions">
          <li class="tile red"></li>
          <li class="tile blue"></li>
          <li class="tile yellow"></li>
          <li class="tile green"></li>
        </ul>
      </div>
    </div>
    <div class="descriptions">
      <div class="info">
        <button v-on:click="startGame" :disabled="isGame">Играть!</button>
        <p v-if="lose">Вы проиграли :(</p>
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
  </div>
</template>

<script>
export default {
  name: 'Simon',
  data: () => {
    return {
      difficulty: "easy",
      playerSteps: [],
      simonsSteps: [],
      lose: false,
      isGame: false,
      step: 0,
    }
  },

  methods: {

    getDifficulty() {   // Simple switcher difficulty
      switch (this.difficulty) {
        case "easy":
          return 1500;
        case "normal":
          return 1000;
        case "hard":
          return 400;
      }
    },

    game() {

      let agreeStep = this.playerSteps[this.step] == this.simonsSteps[this.step]
      let agreeLength = this.step == this.simonsSteps.length - 1

      if (agreeStep) { // If the player repeats Simon's moves, then everything is OK 👌
        this.step += 1
      }
      if (agreeLength && agreeStep) { // If the player repeated Simon's moves
        this.playerSteps = []         // with his own moves, then his sequence
        this.step = 0                 // is cleared, and Simon randomly chooses his next move
        let that = this

        setTimeout(function () {
          return function () {
            that.Simon()
          };
        }(), 1000);

      }
      if (!agreeStep) {     // Game is over 😞

        this.playerSteps = []
        this.simonsSteps = []
        this.isGame = false
        this.lose = true
      }
    },

    actions(e) {
      if (!this.isGame) {  // If the game has not started, then no actions
        return 0
      }
      if (this.isPlayer(e)) {
        this.playSound(e.target.className)
        this.blink(e)
        this.playerSteps.push(e.target.className)
        this.game(e)

      } else {

        let tiles = document.querySelectorAll("li")
        switch (e) {
            // Checks which tile matches his new random move
          case "tile red":
            this.blink(tiles[0]); // And plays an animation with sound
            this.playSound(e)
            break;

          case "tile blue":
            this.blink(tiles[1]);
            this.playSound(e)
            break;

          case "tile yellow":
            this.blink(tiles[2]);
            this.playSound(e)
            break;

          case "tile green":
            this.blink(tiles[3]);
            this.playSound(e)
            break;
        }
      }
    },

    getRandomTile() {     // Simple randomizer to get string of tile for Simon

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

      let soundOne = new Audio(require("./../../sounds/1.mp3"))   // Import sounds
      let soundTwo = new Audio(require("./../../sounds/2.mp3"))
      let soundThree = new Audio(require("./../../sounds/3.mp3"))
      let soundFour = new Audio(require("./../../sounds/4.mp3"))

      let sounds = [soundOne, soundTwo, soundThree, soundFour]

      switch (step) {     // Simple parser for sound's map
        case "tile red":
          sounds[0].play()
          break;

        case "tile blue":
          sounds[3].play()
          break;

        case "tile yellow":
          sounds[2].play()
          break;

        case "tile green":
          sounds[3].play()
          break;

      }
    },

    blink(stepOrEvent) {  // Just simple blinking that uses CSS
      let area

      // Due to the fact that the event
      // is slightly different from
      // the player and Simon, you need
      // to check who wants to make a blink

      if (this.isPlayer(stepOrEvent)) {
        area = stepOrEvent.target;
      } else {
        area = stepOrEvent
      }

      area.style.opacity = "1"
      setTimeout(function () {
        return function () {
          area.style.opacity = "0.6"
        };
      }(), 150);
    },

    isPlayer(e) {         // Checking if it's a player or Simon
      switch (e.target) {
        case undefined:
          return false

        default:
          return true

      }
    },

    startGame() {
      this.isGame = true
      this.lose = false    // Blocking the game start button
      this.step = 0         // Zeroing counters
      this.playerSteps = []
      this.simonsSteps = []

      this.Simon()          // Initializing the game, Simon makes the first move
    },

    Simon() {

      let that = this;
      let bup = that.getRandomTile()
      that.simonsSteps.push(bup)
      let steps = that.simonsSteps

      for (let currentIndex = 0; currentIndex < steps.length; currentIndex++) {

        setTimeout(function () {
          return function () {

            that.actions(steps[currentIndex])

          };
        }(currentIndex), that.getDifficulty() * (currentIndex));
      }
    }
  }
}
</script>

