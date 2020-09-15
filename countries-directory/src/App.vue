<template>
  <div id="app">
    <b-navbar type="light">
      <b-navbar-brand tag="h1" class="ml-4 mt-2">Countries Directory</b-navbar-brand>
    </b-navbar>
    <b-container fluid>
      <b-row>
        <b-col id="filters" cols="12" md="4" lg="3" class="d-md-block">
          <h4 id="filter-header" class="d-none d-md-block">Filter</h4>
          <b-button
            block
            variant="outline-secondary"
            class="d-display d-md-none mb-2"
            ref="btnTextToggle"
            @click="toggleShowFilter"
          >Show filters</b-button>
          <div :class="showFilters ? '' : 'height-zero'">
            <RangeFilter
              :minMaxRange="minMaxPopulation"
              :label="'Population Range:'"
              @range-changed="updatePopulationRange"
            />
            <RangeFilter
              :minMaxRange="minMaxArea"
              :label="'Area Range:'"
              @range-changed="updateAreaRange"
            />
            <DropdownFilter :regions="countryRegions" @dropdown-value-changed="updateRegion" />
          </div>
        </b-col>
        <b-col cols="12" md="8" lg="9" id="countries-col">
          <b-row align-h="center">
            <b-col cols="12" md="6">
              <SearchFilter @search-changed="updateSearchText" />
            </b-col>
            <b-col cols="12" md="3">
              <SortDropdown @sort-value-changed="sortCountries" />
            </b-col>
          </b-row>
          <b-row>
            <b-col cols="12">
              <CountriesList :countries="filteredCountries" />
            </b-col>
          </b-row>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import CountriesList from "./components/CountriesList.vue";
import SearchFilter from "./components/SearchFilter.vue";
import RangeFilter from "./components/RangeFilter.vue";
import DropdownFilter from "./components/DropdownFilter.vue";
import SortDropdown from "./components/SortDropdown.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    CountriesList,
    SearchFilter,
    RangeFilter,
    DropdownFilter,
    SortDropdown,
  },

  data() {
    return {
      countries: [],
      searchText: "",
      minMaxPopulation: [],
      populationRange: [0, 999999999],
      minMaxArea: [],
      areaRange: [0, 9999999999999],
      countryRegions: [],
      filterRegions: [],
      showFilters: false,
      btnTextShow: false,
    };
  },
  computed: {
    // filter countries takes a method which returns true or false
    filteredCountries() {
      let filteredCountries = this.countries.filter(this.countryStartsWith);
      filteredCountries = filteredCountries.filter(
        this.countryPopulationWithinRange
      );
      filteredCountries = filteredCountries.filter(this.countryAreaWithinRange);
      filteredCountries = filteredCountries.filter(this.countryWithinRegion);
      return filteredCountries;
    },
  },
  methods: {
    updateSearchText(text) {
      this.searchText = text.toLowerCase();
    },
    // check if country name starts with string entered in search box
    // return true or false
    countryStartsWith(country) {
      let name = country.name.toLowerCase(); //startsWith is case sensitive
      return name.startsWith(this.searchText);
    },

    countryPopulationWithinRange(country) {
      return (
        country.population < this.populationRange[1] &&
        country.population > this.populationRange[0]
      );
    },

    countryAreaWithinRange(country) {
      return (
        country.area < this.areaRange[1] && country.area > this.areaRange[0]
      );
    },

    countryWithinRegion(country) {
      return this.filterRegions.includes(country.region);
    },

    calcMinMaxPopulation() {
      let minPopulation = 0;
      let maxPopulation = 0;
      if (this.countries) {
        maxPopulation = this.countries[0].population;
        let i;
        for (i = 1; i < this.countries.length; i++) {
          if (this.countries[i].population > maxPopulation) {
            maxPopulation = this.countries[i].population;
          }
        }
      }
      return [minPopulation, maxPopulation];
    },

    calcMinMaxArea() {
      let minArea = 0;
      let maxArea = 0;
      if (this.countries) {
        maxArea = this.countries[0].area;
        let i;
        for (i = 1; i < this.countries.length; i++) {
          if (this.countries[i].area > maxArea) {
            maxArea = this.countries[i].area;
          }
        }
      }
      return [minArea, maxArea];
    },

    getAllRegions() {
      // populate array with all regions
      let countryRegions = [];
      this.countries.forEach((country) => {
        if (!countryRegions.includes(country.region)) {
          countryRegions.push(country.region);
        }
      });
      return countryRegions;
    },

    updatePopulationRange(range) {
      this.populationRange = range;
    },

    updateAreaRange(range) {
      this.areaRange = range;
    },

    updateRegion(value) {
      // if no regions chosen revert back to full list of options
      if (value && value.length > 0) {
        this.filterRegions = value;
      } else {
        this.filterRegions = this.countryRegions;
      }
    },
    compareName(a, b) {
      return a.name.localeCompare(b.name);
    },
    comparePopulation(a, b) {
      return a.population - b.population;
    },
    compareArea(a, b) {
      return a.area - b.area;
    },
    // sort countries based on value emitted from sort dropdown
    sortCountries(selected) {
      let sortBySplit = selected.split("-");
      if (sortBySplit[0] == "name") {
        this.countries.sort(this.compareName);
      } else if (sortBySplit[0] == "pop") {
        this.countries.sort(this.comparePopulation);
      } else if (sortBySplit[0] == "area") {
        this.countries.sort(this.compareArea);
      }
      if (sortBySplit[1] == "reversed") {
        this.countries.reverse();
      }
    },

    toggleShowFilter() {
      this.showFilters = !this.showFilters;
      this.btnTextShow = !this.btnTextShow;
      this.$refs.btnTextToggle.innerText = this.btnTextShow
        ? "Hide Filters"
        : "Show Filters";
    },
  },
  created() {
    //axios used for HTTP requests
    axios
      .get("https://restcountries.eu/rest/v2/all")
      // res is returned from API
      .then((res) => {
        console.log(res);
        this.countries = res.data;
        this.minMaxPopulation = this.calcMinMaxPopulation();
        this.minMaxArea = this.calcMinMaxArea();
        this.countryRegions = this.getAllRegions();
        this.filterRegions = this.countryRegions;
      })
      .catch((err) => console.log(err));
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #758494;
  overflow-y: hidden;
}

nav {
  background-color: #e98074;
}

.navbar-light .navbar-brand {
  color: #505050 !important;
  font-size: 1.4em;
}

#filters {
  padding-left: 70px;
  text-align: left;
  padding-top: 30px;
  padding-bottom: 10px;
}

#filter-header {
  text-align: left;
}

#countries-col {
  max-height: calc(100vh - 75px);
  overflow-y: scroll;
  padding-top: 30px;
}

@media only screen and (max-width: 768px) {
  .height-zero {
    height: 0px;
    overflow: hidden;
  }

  #filters {
    padding-left: 20px;
  }

  #countries-col {
    padding-top: 10px;
  }
}
</style>
 