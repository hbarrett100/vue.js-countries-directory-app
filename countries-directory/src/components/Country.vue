<template>
  <div>
    <b-card :title="country.name" class="country-box" @click="showCountryDetails">
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
            <p>Timezone: {{country.timezones}}</p>
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
</style>