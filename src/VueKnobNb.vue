<template>
<svg width="100%" height="100%" viewBox="0 0 42 42" style="transform: rotate(-90deg);" preserveAspectRatio >
  <circle
          class="hole"
          cx="21"
          cy="21"
          r="15"
          fill="#fff">
  </circle>
  <circle
          ref="ring"
          class="ring"
          cx="21"
          cy="21"
          r="15"
          fill="transparent"
          :stroke="bgcolor"
          stroke-width="5">
     </circle>
  <circle
          ref="segment"
          class="segment"
          cx="21"
          cy="21"
          r="15"
          fill="transparent"
          :stroke="fillcolor"
          stroke-width="5"
          :stroke-dasharray="strokeDasharray"
          stroke-dashoffset="0">
    </circle>
    <g>
    <text y="50%" x="50%" :style="labelStyle">
      {{value}}
    </text>
    </g>
</svg>

</template>
<script>
export default {
  template:'#vue-knob-nb',
  props:{
    value:{
      type:Number,
      required:true,
      validator: (value) => {
         return value > 0 && value <=100
      }
    },
    fillcolor:{
      type:String,
      required:false,
      default: 'black'
      // '#17d'
    },
    bgcolor:{
      type:String,
      required:false,
      default:'#d2d3d4'
    }
  },
  computed:{
    strokeDasharray(){
      //94.2 = 2 * PI * RADIOUS (r="15")
      let value = this.value * 94.2/100;
      return value + ' ' + (94.2-value);
    },
    labelStyle(){
      let transformY;
      if(this.value ===100){
        transformY = 0.75;
      }else{
        if(this.value >= 10){
            transformY = 0.5;
        }else{
            transformY = 0.25;
        }
      }
      return {
        transform: `translateX(-${transformY}em) translateY(0.4em)`,
        'font-size':'0.7em'
      }
    }
  },
  methods: {
    computeValue (e) {
      console.log(e.clientX,e.clientY)
        const rect = this.$refs.ring.getBoundingClientRect(),
            centerX = rect.width / 2 + rect.left,
            centerY = rect.height / 2 + rect.top ,
            clickX = e.clientX,
            clickY = e.clientY;
        let result = Math.atan2(centerY - clickY, centerX - clickX);
        let percentage = (result + Math.PI)/(Math.PI + Math.PI);
        this.$emit('input',Math.ceil(percentage * 100)+25);
    }
  },
  mounted(){
     this.$refs.ring.addEventListener('click',this.computeValue);
     this.$refs.segment.addEventListener('click',this.computeValue);
  }
};
</script>
<style>

</style>