<template>
  <div>
    <b-card class="country-box" @click="showCountryDetails">
      <h4 class="border-bottom pb-2 mb-3">{{ country.name }}</h4>
      <b-media right-align vertical-align="center" class="country-box">
        <template v-slot:aside>
          <b-img :src="country.flag" :alt="country.name + ' flag'" width="120px" height="80px"></b-img>
        </template>
        <b-card-text>
          <div>
            <p>Region: {{country.region}}</p>
            <p>Population: {{country.population}}</p>
            <p>Area: {{country.area}}</p>
          </div>
        </b-card-text>
      </b-media>
      <b-button block variant="primary-outline"><b-icon :icon="show ? 'chevron-compact-up' : 'chevron-compact-down'"/></b-button>
      <b-collapse v-model="show">
        <b-card-text class="country-box">
          <div v-show="show">
            <p>Capital: {{country.capital}}</p>
            <p>Timezones: <span v-for="(timezone, index) in country.timezones" :key="index"><span v-if="index != 0">, </span>{{timezone}}</span></p>
            <p>Currencies: <span v-for="(currency, index) in country.currencies" :key="index"><span v-if="index != 0">, </span>{{currency.name}}</span></p>
            <p>Languages: <span v-for="(language, index) in country.languages" :key="index"><span v-if="index != 0">, </span>{{language.name}}</span></p>
          </div>
        </b-card-text>
      </b-collapse>
    </b-card>
  </div>
</template>

<script>

export default {
  name: "Country",
  props: ["country"],
  data() {
    return {
      show: false,
    };
  },
  methods: {
    showCountryDetails() {
      this.show = !this.show;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.country-box {
  margin: auto;
  width: 80%;
  margin-top: 10px;
}

.card-text {
  text-align: left;
}

.card:hover {
  background-color:#F2F2F2;
  cursor: pointer;
}

@media only screen and (max-width: 768px) {
  .country-box {
  margin: auto;
  width: 90%;
  margin-top: 10px;
}
}
</style>