<template>
  <div id="app">
     <!-- As a heading -->
    <b-navbar variant="info" type="light">
      <b-navbar-brand tag="h1" class="mb-0">Countries Directory</b-navbar-brand>
    </b-navbar>
    <SearchFilter @search-changed="updateSearchText"/>
    <CountriesList :countries="filteredCountries"/>
  </div>
</template>

<script>
import CountriesList from './components/CountriesList.vue'
import SearchFilter from './components/SearchFilter.vue'
import axios from 'axios'


export default {
  name: 'App',
  components: {
    CountriesList,
    SearchFilter
  },
  data() {
  return {
    countries: [],
    searchText: ''
  }
  },
  computed: {
    // filter countries takes a method which returns true or false
    filteredCountries() {
      return this.countries.filter(this.countryStartsWith)
    }
  },
  methods: {
    updateSearchText(text) {
      this.searchText = text.toLowerCase()
    },
    // check if country name starts with string entered in search box
    // return true or false
    countryStartsWith(country) {
      let name = country.name.toLowerCase(); //startsWith is case sensitive
      return name.startsWith(this.searchText);
    }
  },

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
  color: #758494;
}

nav {
  margin-bottom: 30px;
}
</style>
