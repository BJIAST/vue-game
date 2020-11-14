<template>
  <div id="frame">
    <div id="frameControl" :class="{inprocess: ingame}" v-if="!ingame || inprocess">
      <button id="startGame" @click="gameStart" v-if="!ingame"> START</button>
    </div>
    <div id="frameGame">
      <div v-for="color in colors" class="card" :color="color" @click="showColor($event)"></div>
    </div>
  </div>
</template>

<script>

export default {
  name: "GameFrame",
  props: ['colors'],
  data() {
    return {
      ingame: false,
      inprocess: false,
      moveCards: []
    }
  },
  mounted() {

  },
  methods: {
    gameStart() {
      this.ingame = true;

      this.$emit('gameStart', this.ingame);
    },
    showColor(event) {
      const elem = event.target;
      this.moveCards.push(elem);

      elem.style.background = elem.getAttribute('color');

      if (this.moveCards.length >= 2) {
        this.inprocess = true;
        const cards = this.moveCards;

        if (!this.checkMoveResult()) {
          setTimeout(() => {
            cards.forEach(card => card.style.background = 'none');
          }, 600)
        }

        this.moveCards = [];
        setTimeout(() => this.inprocess = false, 700);

        this.$emit('gameMove', true);
      }
    },
    checkMoveResult() {
      return this.moveCards[0].getAttribute('color') === this.moveCards[1].getAttribute('color');
    }
  }
}
</script>

<style scoped>

#frame {
  width: 100%;
}

#frameControl {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 3;
  background: rgba(0, 0, 0, .3);
}

#frameControl.inprocess {
  background: none;
}

#startGame {
  background-color: #06000e;
  font-weight: 700;
  color: #fff;
  text-align: center;
  vertical-align: middle;
  border-radius: 8px;
  border: 2px solid #fff;
  font-size: 25px;
  display: inline-block;
  padding: 5%;
  cursor: pointer;
  transition: .7s;
  outline: none;
}

#startGame:hover {
  background-color: #2d2d2d;
}

#startGame:focus {
  background-color: #5b5b5b;
}

#frameGame {
  display: flex;
  justify-content: space-between;
  flex-flow: row wrap;
}

#frameGame .card {
  flex-basis: calc(100% / 4 - 5px);
  height: calc(80vh / 4 - 5px - 30px);
  margin-bottom: 5px;
  border: 1px solid #000;
  border-radius: 4px;
  transition: .7s;
  cursor: pointer;
  background: none;
}

#frameGame .card:hover {
  border-color: #bebebe;
}

</style>