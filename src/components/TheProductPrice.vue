<template>
    <span>{{prefix+parseFloat(value).toFixed(precision)}}</span>
</template>
<script>

export default ({
    name: "TheProductPrice",
    props: {
        value: Number
    },
    data() {
        return {
            prefix: '$',
            precision: 2
        }
    }
})
</script>
