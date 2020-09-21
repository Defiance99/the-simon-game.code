<template>
  <div id="app">
    <TheSimonGame></TheSimonGame>
  </div>
</template>

<script>
import TheSimonGame from './components/simon-game.vue'

export default {
  name: 'App',
  components: {
    TheSimonGame
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
