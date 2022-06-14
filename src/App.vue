<template>
  <div id="app">
    <Header
      :pickedColor="pickedColor"
      :headerStyleBackgroundColor="headerStyleBackgroundColor"
    />
    <Navbar
      :isHard="isHard"
      :colorCount="colorCount"
      :init="init"
      :messageDisplay="messageDisplay"
      :restartButton="restartButton"
      @isHard="isHard = $event"
      @colorCount="colorCount = $event"
    />
    <GameContainer :colors="colors" :handlerSquareClick="handlerSquareClick" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Navbar from "./components/NavBar.vue";
import GameContainer from "./components/GameContainer.vue";
export default {
  name: "App",
  components: {
    Header,
    Navbar,
    GameContainer,
  },
  mounted() {
    this.init();
  },
  data() {
    return {
      colorCount: 6,
      isHard: true,
      colors: [],
      pickedColor: "",
      restartButton: "New Colors!",
      headerBackgroundColor: "",
      messageDisplay: "",
      msgDisplayStyleColor: "",
      headerStyleBackgroundColor: "steelblue",
    };
  },
  methods: {
    pickColor() {
      var quantity;
      if (this.isHard) {
        quantity = 6;
      } else {
        quantity = 3;
      }
      return Math.floor(Math.random() * quantity);
    },
    init() {
      this.messageDisplay = "";
      this.restartButton = "New Colors!";
      this.headerStyleBackgroundColor = "steelblue";
      this.colors = this.createNewColors(this.colorCount);
      this.pickedColor = this.colors[this.pickColor()];
    },
    createNewColors(numbers) {
      let arr = [];
      for (let i = 0; i < numbers; i++) {
        arr.push(this.createRandomStringColor());
      }
      return arr;
    },
    createRandomStringColor() {
      let newColor =
        "rgb(" +
        this.randomInt() +
        ", " +
        this.randomInt() +
        ", " +
        this.randomInt() +
        ")";
      return newColor;
    },
    randomInt() {
      return Math.floor(Math.random() * 256);
    },
    handlerSquareClick({ target }) {
      let clickedColor = target.style.backgroundColor;
      if (clickedColor === this.pickedColor) {
        this.messageDisplay = "You Picked Right!";
        this.setAllColorsTo(this.pickedColor);
        this.restartButton = "Play Again!";
        this.headerStyleBackgroundColor = this.pickedColor;
      } else {
        target.style.backgroundColor = "#232323";
        this.messageDisplay = "Try Again!";
        this.msgDisplayStyleColor = "#000000";
      }
    },
    setAllColorsTo(selectedColor) {
      this.colors = this.colors.map(() => selectedColor);
    },
  },
};
</script>

<style>
body {
  background: #232323;
  margin: 0;
  font-family: "Montserrat", "Avenir";
}

h1 {
  font-weight: normal;
  line-height: 1.1;
  padding: 20px 0;
}
</style>
