<template>
  <div id="app">
    <h1>2次元3自由度マニピュレータ</h1>
    <Arm/>
  </div>
</template>

<script>
import Arm from './components/Arm.vue'

export default {
  name: 'App',
  components: {
    Arm
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
  margin-top: 60px;
}
</style>
