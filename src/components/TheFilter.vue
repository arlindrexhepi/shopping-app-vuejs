<template>
    <div>
    <button class="btn-filter" @click="$emit('toggleFilter')">{{textString}} <strong>&dollar;</strong></button>
    <div v-if="rangestatus">
      <input type="number" :value="modelValue" @input="$emit('update:modelValue', $event.target.value)">
      <input type="range" min="0" max="1000" :value="modelValue" @input="$emit('update:modelValue', $event.target.value)"> 
    </div>
    </div>
</template>

<script>
    export default {
        name: 'TheFilter',
        emits: ['toggleFilter'],
        props: {
            modelValue: Number,
            textString: String,
            rangestatus: Boolean,
        },
        data() {
            return {
                
            }
        },
        methods: {
            
        }
    }
</script>

<style scoped>

</style>