<template>
  <div>
    <div class="game">
      <div class="simon">
        <ul v-on:click="playSound">
          <li class="tile red"></li>
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
      <input type="radio" v-model="difficulty" name="mode" value="easy" id="easy" checked><label for="easy">Лёгкая</label><br>
      <input type="radio" v-model="difficulty" name="mode" value="normal" id="normal"><label for="normal">Средняя</label><br>
      <input type="radio" v-model="difficulty" name="mode" value="hard" id="hard"><label for="hard">Сложная</label>
    </div>
  </div>
</template>

<script>

let soundOne = new Audio(require("./../../sounds/1.mp3"))
let soundTwo = new Audio(require("./../../sounds/2.mp3"))
let soundThree = new Audio(require("./../../sounds/3.mp3"))
let soundFour = new Audio(require("./../../sounds/4.mp3"))

function getRandomInRange(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}


export default {
  name: 'Simon',
  data: () => {
    return {
      difficulty: "easy"
    }
  },
  methods: {
    playSound(e) {
      switch (e.target.className) {
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
    startGame() {

    }
  }
}
</script>

