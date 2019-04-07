<template>
  <div id="app">
    <meteo/>
  </div>
</template>

<script>
import meteo from './components/meteo.vue'
import VueFilterDateFormat from 'vue-filter-date-format';


export default {
  name: 'app',
  components: {
    meteo
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.logo{
	display: block;
	margin: 20px auto;
	height: 75px;
}
</style>
