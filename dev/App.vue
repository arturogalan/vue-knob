<template>
    <div style="height:40vw;width:40vw;margin:auto;">
        <vue-knob-nb v-model="myVal" fillcolor="black" bgcolor="grey"></vue-knob-nb>
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