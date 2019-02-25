<template>
    <div class="score" v-if="player">
        {{ score }}
    </div>
</template>

<script>
export default {
  name: 'score',
  props: ['score', 'player']
}
</script>

<style scoped>
    .score {
        display: inline-block;
        height: 50px;
        width: 150px;
        font-size: 17pt;
        font-weight: 500;
        color: greenyellow;
        background-color: #666;
        line-height: 50px;
        padding: 5px;
        margin: 0 auto;
    }
</style>
