<template>
  <div id="app">
    <spin-win></spin-win>
  </div>
</template>

<script>
import SpinWin from './components/SpinWin.vue'

export default {
  name: 'App',
  components: {
    SpinWin,
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 60px;
}
</style>
