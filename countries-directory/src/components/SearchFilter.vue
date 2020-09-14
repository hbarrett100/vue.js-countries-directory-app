
<template>
  <div>
    <b-form-input v-model="text" placeholder="Search country name.." @keyup="$emit('search-changed', text)"></b-form-input>
  </div>
</template>

<script>
  export default {
    name: 'SearchFilter',
    data() {
      return {
        text: ''
      }
    }

  }
</script>

<style scoped>
input {
  width: 50%
}
</style>