<template>
  <div ref="sidePanel"></div>
  <Map></Map>
</template>

<script>
  import Map from './components/MapContainer.vue'
  export default {
    name: 'App',
    components: {
      Map
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
  
  html, body, #map, #app {
    height: 100%;
    margin: 0;
    padding: 0;
  }
</style>