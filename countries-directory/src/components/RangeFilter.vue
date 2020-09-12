<template>
  <div>
    <VueSimpleRangeSlider
                    style="width: 300px"
                    :min="minMaxRange[0]"
                    :max="minMaxRange[1]"
                    :logarithmic="true"
                    v-model="range"
                    @input="$emit('range-changed', range)"
            />
  </div>
</template>
<script>
import VueSimpleRangeSlider from 'vue-simple-range-slider';
import 'vue-simple-range-slider/dist/vueSimpleRangeSlider.css'
export default {
    // register the component
    components: { VueSimpleRangeSlider },
    data() {
          return {
               range: []
          }
    },
    props: ["minMaxRange"],
    watch: {
    minMaxRange: function() {
      this.range = this.minMaxRange
    }
    }
}
</script> 

