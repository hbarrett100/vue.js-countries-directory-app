<template>
  <div>
    <div class="country-box" @click="showCountryDetails">
      <h4>{{country.name}}</h4>
      <div v-show="show">
            <p>Region: {{country.region}}</p>
            <p>Population: {{country.population}}</p>
            <p>Captial: {{country.capital}}</p>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Country",
  props: ["country"],
  data() {
      return {
          show: false
      }
  },
  methods: {
        showCountryDetails() {
            this.show= !this.show;
        }
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.country-box {
  border: 2px solid;
  margin: auto;
  margin-bottom: 5px;
  width: 60%;
  
}
</style>