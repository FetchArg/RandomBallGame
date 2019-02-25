<template>
    <div class="content">
        <div class="game" v-on:click="clickOnInterface" :class="{wait: !player || stopped}">
            <span class="time" v-if="!stopped">{{ time }}</span>
            <span v-if="player && !stopped" class="round" :style="roundStyle" :class="{ bonus: bonusActivated, penalty: penaltyActivated }" @click.stop="clickOnRound" v-on:click.alt.stop="bonus"></span>
        </div>
        <div class="log text-center">
            <p v-for="item in userLogs">
                #{{ item.id }} - {{ item.message }}
            </p>
        </div>
    </div>
</template>

<script>
export default {
  name: 'game',
  props: ['player'],
  data: function () {
    return {
      click: 0,
      time: 0,
      roundStyle: {
        height: '50px',
        width: '50px',
        margin: '20% 20%'
      },
      bonusActivated: false,
      penaltyActivated: false,
      collection: [],
      stopped: true
    }
  },
  computed: {
    userLogs: function () {
      return this.collection.filter(function (item) {
        return item.type === 'user'
      })
    }
  },
  created: function () {
    document.onkeydown = this.start
  },
  watch: {
    click: function () {
      this.updateRound()
      this.$emit('score', this.click)
    },
    player: function () {
      this.stopped = false
      this.time = 20
      let self = this
      setInterval(function () {
        self.updateTime()
      }, 1000)
    }
  },
  methods: {
    updateTime: function () {
      if (this.time === 0) {
        this.stopped = true
      }

      if (!this.stopped) {
        this.time--
      }
    },
    clickOnRound: function (event) {
      setTimeout(this.updateRound, 1000)

      this.updateClick(true)
      this.addLog(`BRAVO !`)
    },
    bonus: function (event) {
      if (this.bonusActivated) {
        this.click++
        this.addLog(`PERFECT +2`)
      } else {
        this.click--
        this.addLog(`????? -1`)
      }
    },
    clickOnInterface: function (event) {
      this.click--
      this.addLog(`HO NON :( -1`)
    },
    updateClick: function (increment) {
      if (!this.player || this.stopped) {
        return
      }

      if (increment) {
        this.click++
      } else {
        this.click--
      }
    },
    start: function (event) {
      if (event.key === 'Enter') {
        console.log('START')
      }
    },
    updateRound: function () {
      let size = Math.random() * (100 - 10) + 10
      let top = Math.random() * (60 - 5) + 5
      let left = Math.random() * (60 - 5) + 5

      this.bonusActivated = size > 80
      this.penaltyActivated = size < 20

      this.addLog({
        size: size,
        top: top,
        left: left
      }, 'round')

      this.roundStyle.height = this.roundStyle.width = `${size}px`
      this.roundStyle.margin = `${top}% ${left}%`
    },
    addLog: function (message, type) {
      if (!this.player || this.stopped) {
        return
      }

      let typeOfMessage = type || 'user'
      this.collection.unshift({id: this.collection.length / 2, message: message, type: typeOfMessage})
    }
  }
}
</script>

<style scoped>
    .content {
        height: 400px;
    }

    .log {
        width: 100%;
        height: 30px;
        background: #666;
        display: block;
        overflow: hidden;
        padding: 6px;
    }

    .game {
        width: 100%;
        height: 90%;
        display: block;
        background: #000000;
        opacity: 1;
        transition: opacity 1s;
    }

    .round {
        background: aliceblue;
        border-radius: 9999px;
        position: absolute;
        transition: width 2s, height 2s, margin 0.5s;
    }

    .bonus {
        background: #1f1514;
    }

    .penalty {
        background: #271a19;
    }

    .wait {
        opacity: 0.3
    }

    .time {
        position: absolute;
        font-size: 100pt;
        padding-left: 30px;
        color: darkgoldenrod;
        opacity: 0.2;
    }
</style>
