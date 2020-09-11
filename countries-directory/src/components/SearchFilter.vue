
<template>
  <div>
    <b-form-input v-model="text" placeholder="Enter country name" @keyup="$emit('search-changed', text)"></b-form-input>
  </div>
</template>

<script>
  export default {
    name: 'SearchFilter',
    data() {
      return {
        text: ''
      }
    },
    computed() {
      this.searchText = this.text

    }
  }
</script>