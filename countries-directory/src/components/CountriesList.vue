<template>
  <div>
        <div v-for="country in countries" :key="country.name">
            <Country :country="country" />
        </div>
  </div>
</template>

<script>
import Country from './Country.vue'

export default {
  name: 'CountriesList',
   components: {
    Country
  },
  props: ["countries"]

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
