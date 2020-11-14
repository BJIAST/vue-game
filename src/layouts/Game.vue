<template>
  <div id="game">
    <Dashboard :gameStatus="ingame" :moves="moves"></Dashboard>
    <GameFrame :colors="colors" @gameStart="handleGameStart" @gameMove="handleMove"></GameFrame>
  </div>
</template>
<script>
import Dashboard from "@/components/Dashboard";
import GameFrame from "@//components/GameFrame";
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
      colors: ['lightblue', 'darkgreen', 'black', 'grey', 'white', 'purple', 'darkred', 'blue']
    }
  },
  methods: {
    handleGameStart(data) {
      this.ingame = data;
    },
    handleMove() {
      this.moves += 1;
    }
  },
  mounted() {
    this.colors.push(...this.colors);
    this.colors.sort(() => Math.random() - 0.5);
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