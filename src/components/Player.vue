<template>
    <div>
        <span v-html="welcomeMessage" :class="{ hide: !player }"></span>

        <form @submit.prevent="setPlayer" :class="{ hide: player }">
            <input name="player" placeholder="Entrez votre nom de joueur...">
            <button type="submit">Jouer</button>
        </form>
    </div>
</template>

<script>
export default {
  name: 'player',
  data: function () {
    return {
      player: '',
      welcomeMessage: ''
    }
  },
  updated: function () {
    this.welcomeMessage = `Bonjour <span class="player">${this.player}</span> !`
  },
  methods: {
    setPlayer: function (event) {
      let playerName = event.target[0].value

      if (!playerName) {
        alert('Merci de renseigner votre nom de joueur !')

        return
      }

      this.player = playerName
      this.$emit('player')
    }
  }
}
</script>

<style scoped>
    .hide {
      display: none;
    }
</style>
