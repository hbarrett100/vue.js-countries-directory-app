<template>
  <div id="app">
     <!-- As a heading -->
    <b-navbar variant="info" type="light">
      <b-navbar-brand tag="h1" class="mb-0">Countries Directory</b-navbar-brand>
    </b-navbar>
    <CountriesList :countries="countries"/>
  </div>
</template>

<script>
import CountriesList from './components/CountriesList.vue'
import axios from 'axios'


export default {
  name: 'App',
  components: {
    CountriesList
  },
  data() {
  return {
    countries: []
  }
  },
  methods: {},

  created() {
  //axios used for HTTP requests
  axios.get('https://restcountries.eu/rest/v2/all')
  // res is returned from API
  .then(res => {console.log(res);
                this.countries = res.data})
  .catch(err => console.log(err))
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
}

nav {
  margin-bottom: 30px;
}
</style>
