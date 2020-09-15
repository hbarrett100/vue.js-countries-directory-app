<template>
  <div>
    <label class="typo__label">Select Region(s):</label>
    <multiselect
      v-model="value"
      @input="$emit('dropdown-value-changed', value)"
      :options="options"
      :multiple="true"
      :close-on-select="false"
      :clear-on-select="false"
      :preserve-search="true"
      placeholder="-- Select --"
      :preselect-first="true"
    >
      <template slot="selection" slot-scope="{ values, search, isOpen }">
        <span
          class="multiselect__single"
          v-if="values.length &amp;&amp; !isOpen"
        >{{ values.length }} options selected</span>
      </template>
    </multiselect>
  </div>
</template>

<script>
import Multiselect from "vue-multiselect";

export default {
  name: "DropdownFilter",
  components: {
    Multiselect,
  },
  props: ["regions"],
  data() {
    return {
      value: [],
      options: [],
    };
  },
  watch: {
    // first value in watch fn is new value of prop
    regions: function (newRegions) {
      this.options = newRegions;
    },
  },
};
</script> 
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<style scoped>
.multiselect {
  width: 250px;
}
</style>