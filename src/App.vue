<template>
  <div id="app">
    <img src="./assets/logo.png">
    <mainpage/>
  </div>
</template>

<script>
import mainpage from './components/mainpage'

export default {
  name: 'show-orders',
  components: {
    mainpage
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
