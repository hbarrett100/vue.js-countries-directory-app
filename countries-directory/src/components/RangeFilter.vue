<template>
  <div>
    <VueSimpleRangeSlider
                    style="width: 300px"
                    :min="minMaxPopulation[0]"
                    :max="minMaxPopulation[1]"
                    :logarithmic="true"
                    v-model="range"
                    @input="$emit('population-range-changed', range)"
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
               range: [0, 1377422166]
          }
    },
    props: ["minMaxPopulation"],
    methods: {

    }
}
</script> 

