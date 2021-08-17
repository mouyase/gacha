<template>
  <div id="app">
    <Gacha/>
  </div>
</template>

<script>
import Gacha from './components/Gacha.vue'

export default {
  name: 'App',
  components: {
    Gacha
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
