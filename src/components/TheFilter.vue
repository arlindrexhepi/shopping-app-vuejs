<template>
    <div class="animate__animated animate__fadeInDownBig my-4">
    <button class="btn-filter" @click="$emit('toggleFilter')">{{textString}} <font-awesome-icon icon="dollar-sign"></font-awesome-icon></button>
    <transition enter-active-class="animate__animated animate__fadeInDown" leave-active-class="animate__animated animate__fadeOutUp" mode="out-in">
      <div v-show="rangestatus" class="my-4">
        <label for="forMax" class="mx-2">Maximum</label>
        <input id="forMax" type="number" :style="{maxWidth: inputWidth + 'px', borderRadius: borderRadius + 'px'}" :value="modelValue" @input="$emit('update:modelValue', $event.target.value)">
        <input type="range" class="form-range mx-2" :style="{maxWidth: rangeWidth + 'px'}" min='0' max='1000' :value="modelValue" @input="$emit('update:modelValue', $event.target.value)"> 
      </div>
    </transition>
    </div>
</template>

<script>
    import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
    export default {
        name: 'TheFilter',
        emits: ['toggleFilter'],
        components: {
          FontAwesomeIcon
        },
        props: {
            modelValue: Number,
            textString: String,
            rangestatus: Boolean,
        },
        data() {
            return {
              inputWidth: 80,
              rangeWidth: 150,
              borderRadius: 7,
            }
        },
        methods: {
            
        }
    }
</script>

<style scoped>

</style>