<template>
  <div class="container" id="app">
    <StockCalc />
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import StockCalc from './components/stockCalc/StockCalc.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    StockCalc,
  }
}
</script>

<style lang="scss">
$bg:'./components/stockCalc/StockCalc-bg.jpg';

*{
  box-sizing: border-box;
}

body{
  background: url($bg) center center no-repeat;
  background-size: cover;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0 20px;
  padding-bottom: 250px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}

.container{
  max-width: 970px;
}

</style>
