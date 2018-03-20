<template>
  <div id="app">
    <!-- <div class="jumbotron logo">
       <img src="./assets/logo.png">
    </div>   -->
    <hello></hello>
  </div>
</template>

<script>
import Hello from './components/HelloWorldKids'

export default {
  name: 'app',
  components: {
    Hello
  }
}
</script>

<style>

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 30px;
}

#app .logo{
  text-align: center;
}
</style>
