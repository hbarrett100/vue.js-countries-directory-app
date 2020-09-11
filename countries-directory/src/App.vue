<template>
  <div id="app">
     <!-- As a heading -->
    <b-navbar variant="info" type="light">
      <b-navbar-brand tag="h1" class="mb-0">Countries Directory</b-navbar-brand>
    </b-navbar>
    <SearchFilter @search-changed="updateSearchText"/>
    <RangeFilter :minMaxPopulation="minMaxPopulation" @population-range-changed="updatePopulationRange"/>
    <CountriesList :countries="filteredCountries"/>
  </div>
</template>

<script>
import CountriesList from './components/CountriesList.vue'
import SearchFilter from './components/SearchFilter.vue'
import RangeFilter from './components/RangeFilter.vue'
import axios from 'axios'


export default {
  name: 'App',
  components: {
    CountriesList,
    SearchFilter,
    RangeFilter
  },
  data() {
  return {
    countries: [],
    searchText: '',
    minMaxPopulation: [],
    populationRange: [0, 999999999]

  }
  },
  computed: {
    // filter countries takes a method which returns true or false
    filteredCountries() {
      let filteredCountries = this.countries.filter(this.countryStartsWith)
      filteredCountries = filteredCountries.filter(this.countryPopulationWithinRange)
      return filteredCountries
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
    },

    countryPopulationWithinRange(country) {
      return country.population < this.populationRange[1] && country.population > this.populationRange[0]
    },

    calcMinMaxPopulation() {
    let minPopulation = 0;
    let maxPopulation = 0;
    if (this.countries) {
    minPopulation = this.countries[0].population;
    maxPopulation = this.countries[0].population;
    let i;
    for (i = 1; i < this.countries.length; i++) {
      if (this.countries[i].population < minPopulation) {
        minPopulation = this.countries[i].population
      }
      if (this.countries[i].population > maxPopulation) {
      maxPopulation = this.countries[i].population
      }
    }
    }
    return [minPopulation, maxPopulation];
  },

  updatePopulationRange(range) {
    this.populationRange = range;
  }
  
  },

  created() {
  //axios used for HTTP requests
  axios.get('https://restcountries.eu/rest/v2/all')
  // res is returned from API
  .then(res => {console.log(res);
                this.countries = res.data;
                this.minMaxPopulation = this.calcMinMaxPopulation()})
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
