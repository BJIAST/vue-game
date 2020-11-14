<template>
  <div id="game">
    <Dashboard :gameStatus="ingame" :moves="moves" @resetGame="handleResetGame"></Dashboard>
    <GameFrame
        :gameStatus="ingame"
        :colors="cards"
               @gameStart="handleGameStart"
               @gameMove="handleMove"
               @foundedCards="handleOpened">
    </GameFrame>
  </div>
</template>
<script>
import Dashboard from "@/components/Dashboard";
import GameFrame from "@/components/GameFrame";

export default {
  name: "Game",
  components: {
    Dashboard,
    GameFrame,
  },
  data() {
    return {
      ingame: false,
      moves: 0,
      openedCards: [],
      cards: ['lightblue', 'darkgreen', 'black', 'grey', 'white', 'purple', 'darkred', 'blue']
    }
  },
  watch: {
    openedCards(val) {
      if (val.length === this.cards.length / 2) {
        alert(`Game finished!`);
        this.reset();
      }
    }
  },
  methods: {
    handleGameStart(data) {
      this.ingame = data;
    },
    handleMove() {
      this.moves += 1;
    },
    handleOpened(data) {
      this.openedCards.push(data)
    },
    handleTimer(data) {
      this.timer = data;
    },
    handleResetGame(){
      this.reset();
    },
    reset(){
      this.ingame = false;
      this.moves = 0;
      this.openedCards = [],
      this.shuffle(this.cards);
    },
    shuffle(array) {
      array.sort(() => Math.random() - 0.5);
    }
  },
  mounted() {
    this.cards.push(...this.cards);
    this.shuffle(this.cards);
  }
}
</script>

<style scoped>
#game {
  max-width: 75%;
  background: #e0e0e0;
  height: 80vh;
  margin: 10vh auto;
  padding: 40px;
  box-sizing: border-box;
}
</style>