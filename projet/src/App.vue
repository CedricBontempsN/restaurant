<template>
  <div id="app">
    <ListeRestaurants/>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import ListeRestaurants from "@/components/ListeRestaurants"

export default {
  name: 'App',
  components: {
    ListeRestaurants
    //HelloWorld
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
