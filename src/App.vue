<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/main-logo.svg">
    <TozStore />
  </div>
</template>

<script>
import TozStore from './components/TozStore.vue'

export default {
  name: 'app',
  components: {
    TozStore
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
