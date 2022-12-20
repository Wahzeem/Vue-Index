<template>
  <div id="app" class="container">
    <Passengers /> <!--Passengers Component within App-->
  </div>
</template>

<script>

import Passengers from './components/Passengers.vue' //import my component

export default {
  name: 'App',
  components: {
    Passengers
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
