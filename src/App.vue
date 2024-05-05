<template>
  <!-- <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav> -->
  <router-view/>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.min.css'
import '@fortawesome/fontawesome-free/css/all.min.css'
export default {
  data () {
    return {}
  },
  created () {}
}
</script>

<style>
*{
  box-sizing: border-box;
}
body {
  padding: 0;
  height: 100vh;
  /* overflow: hidden; */
  background-color: #FFD541;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

}
</style>
