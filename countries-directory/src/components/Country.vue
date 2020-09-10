<template>
  <div>
        <div>
            <p> {{country.name}} </p>
        </div>
  </div>
</template>

<script>
export default {
  name: 'Country',
  props: ["country"]

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>