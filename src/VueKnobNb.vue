<template>
<div>
<svg width="100%" height="100%" viewBox="0 0 42 42" style="transform: rotate(-90deg);" preserveAspectRatio >
  <circle
    class="hole"
    cx="21"
    cy="21"
    r="15"
    :fill="fillcolor">
  </circle>
  <circle 
    ref="ring"
    class="ring"
    cx="21"
    cy="21"
    r="15"
    fill="transparent"
    :stroke="bgcolor"
    stroke-width="5"
    @click="computeValue">
  </circle>
  <circle
    ref="segment"
    class="segment"
    cx="21"
    cy="21"
    r="15"
    fill="transparent"
    :stroke="barcolor"
    stroke-width="5"
    :stroke-dasharray="strokeDasharray"
    stroke-dashoffset="0"
    @click="computeValue">
  </circle>
  <g transform = "rotate(90 21 21)">
    <text ref="textVal" y="50%" x="50%" :style="labelStyle" @click="computeValue">
      {{currentValue}}
    </text>
  </g>

</svg>
</div>
</template>
<script>
export default {
  template:'#vue-knob-nb',
  props:{
    initValue:{
      type:Number,
      required:true,
      validator: (initValue) => {
         return initValue > 0 && initValue <=100
      }
    },
    fillcolor:{
      type:String,
      required:false,
      default: 'red'
      // '#17d'
    },
    barcolor:{
      type:String,
      required:false,
      default: 'black'
      // '#17d'
    },
    bgcolor:{
      type:String,
      required:false,
      default:'#d2d3d4'
    },
  },
  data() {
    return {
      radius: 15,
      currentValue: this.initValue
    };
  },
  computed:{
    circumference(){ 
      return this.radius * 2 * Math.PI
    },
    strokeDasharray(){
      //94.2 = 2 * PI * RADIOUS (r="15")
      // console.log(this.circumference)
      let value = this.currentValue * this.circumference/100;
      return value + ' ' + (this.circumference-value);
    },
    labelStyle(){
      let transformY;
      if(this.currentValue ===100){
        transformY = 0.75;
      }else{
        if(this.currentValue >= 10){
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
        const rect = this.$refs.ring.getBoundingClientRect(), 
            centerX = rect.width / 2 + rect.left,
            centerY = rect.height / 2 + rect.top ,
            clickX = e.clientX,
            clickY = e.clientY;
        let result = Math.atan2(centerY - clickY, centerX - clickX);
        let percentage = (result + Math.PI)/(Math.PI + Math.PI) * 100;
        let adjustPercentage = percentage + 25 > 100 ? percentage + 25 - 100 : percentage + 25; 
        this.currentValue = Math.ceil(adjustPercentage);
    }
  },
  mounted(){
  }
};
</script>
<style>
</style>