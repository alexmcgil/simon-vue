<template>
  <div id="app">
    <h1>Simon vue</h1>
    <Simon/>
  </div>
</template>

<script>
import Simon from './components/Simon.vue'

export default {
  name: 'App',
  components: {
    Simon
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
}
</style>
