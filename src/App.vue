<template>
  <div id="app">
    <p>2次元3自由度マニピュレータ</p>
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
