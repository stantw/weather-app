<template id="1">
  <div id="app">
    <header>
      <h1>The Weather App</h1>
    </header>
    <body>
      <WeatherComponent />
    </body>
  </div>
</template>

<script>
// Import the WeatherComponent
import WeatherComponent from './components/WeatherComponent.vue';

export default {
  name: 'App',
  components: {
    WeatherComponent // Register the WeatherComponent for use in this template
  }
}
</script>

<style>
#app {
  text-align: center;
}

header {
  text-align: left;
  padding: 0px;
  margin-bottom: 10px;
}

body {
  background: url('~@/assets/background-img.png') no-repeat center center fixed;
  background-size: cover; 
  padding: 0 20px;
}


</style>
