<template>
    <font-awesome-icon icon="dollar-sign"></font-awesome-icon><span>{{parseFloat(value).toFixed(precision)}}</span>
</template>
<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

export default ({
    name: "TheProductPrice",
    components: {
      FontAwesomeIcon,
    },
    props: {
        value: Number
    },
    data() {
        return {
            precision: 2
        }
    }
})
</script>
