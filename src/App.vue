<template>
  <div id="app">
    <h1 class="text-center">Random Ball Game</h1>
    <game :player="player" v-on:score="updateScore"/>
    <player v-on:player="updatePlayer" class="text-center"/>
    <score :score="score" :player="player" class="text-center"/>
  </div>
</template>

<script>
import Game from './components/Game'
import Score from './components/Score'
import Player from './components/Player'

export default {
  name: 'App',
  data: function () {
    return { score: 0, player: false }
  },
  components: {
    Game,
    Score,
    Player
  },
  methods: {
    updateScore: function (score) {
      this.score = score
    },
    updatePlayer: function () {
      this.player = true
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 700px;
  display: block;
}
</style>
