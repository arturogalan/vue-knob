<template>
    <div style="height:40vw;width:40vw;margin:auto;">
    <!-- <div style="height:300px;width:300px;margin:auto;"> -->
        <vue-knob-nb :initValue="myVal" barcolor="black" fillcolor="red" bgcolor="grey"></vue-knob-nb>
    </div>
</template>
<script>
    import VueKnobNb from '../src/index'
    export default {
        components: {VueKnobNb},
        data () {
            return {
                myVal:5
            }
        }
    }
</script>