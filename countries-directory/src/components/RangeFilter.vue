<template>
  <div>
    <label>{{ label }}</label>
    <VueSimpleRangeSlider
      style="width: 250px"
      :min="minMaxRange[0]"
      :max="minMaxRange[1]"
      barColor="#E98074"
      activeBarColor="#E98074"
      :logarithmic="true"
      v-model="range"
      @input="$emit('range-changed', range)"
    />
  </div>
</template>
<script>
import VueSimpleRangeSlider from "vue-simple-range-slider";
import "vue-simple-range-slider/dist/vueSimpleRangeSlider.css";
export default {
  // register the component
  components: { VueSimpleRangeSlider },
  data() {
    return {
      range: [],
    };
  },
  props: ["minMaxRange", "label"],

  //watch for change in minMaxRange prop
  watch: {
    minMaxRange: function () {
      this.range = this.minMaxRange;
    },
  },
};
</script> 

<style scoped>
label {
  margin-bottom: 15px;
  text-align: left;
  font-size: 1.1em;
}
</style>

