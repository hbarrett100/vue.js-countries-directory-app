<template>
  <div>
    <!-- <div class="country-box" @click="showCountryDetails"> -->



      <div>
        <b-card :title="country.name" class="country-box" @click="showCountryDetails">
          <b-card-text>
            <img :src="country.flag" :alt="country.name + ' flag'" width="20px" height="20px"/>
            <div>
                  <p>Region: {{country.region}}</p>
                  <p>Population: {{country.population}}</p>
                  <p>Area: {{country.area}}</p>
              </div>
              <div v-show="show">
                More info.. 
              </div>
          </b-card-text>


         
        </b-card>
      </div>
      <!-- <b-card class="country-box" @click="showCountryDetails">
        <b-card-text>
           <h4>{{country.name}} <img :src="country.flag" :alt="country.name + ' flag'" width="20px" height="20px" /></h4>
            <div v-show="show">
                  <p>Region: {{country.region}}</p>
                  <p>Population: {{country.population}}</p>
                  <p>Captial: {{country.capital}}</p>
                  
              </div>
         
        </b-card-text>
      </b-card> -->

     
    </div>
  <!-- </div> -->
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
  margin: auto;
  margin-bottom: 5px;
  width: 50%;


}
</style>