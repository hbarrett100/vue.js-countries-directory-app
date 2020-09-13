<template>
  <div>
    <b-form-select v-model="selected" :options="options" @change="$emit('sort-value-changed', selected)"></b-form-select>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        selected: null,
        options: [
          { value: null, text: 'Sort By' },
          { value: 'name', text: 'Country name a-z' },
          { value: 'name-reversed', text: 'Country name z-a' },
          { value: 'pop', text: 'Population low to high' },
          { value: 'pop-reversed', text: 'Population high to low'},
          { value: 'area', text: 'Area low to high'},
          { value: 'area-reversed', text: 'Area high to low'},
        ]
      }
    }
  }
</script>