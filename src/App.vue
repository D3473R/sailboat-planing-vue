<template>
  <div id="app">
    <Leaflet msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Leaflet from './components/Leaflet.vue'

export default {
  name: 'app',
  components: {
    Leaflet
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  width: 100vw;
  height: 100vh;
}
</style>
