<template>
    <div v-for="(product, index) in filteredProductsByPrice" :key="index">
      <div v-if="product.price<=Number(priceRange)">
        <h2 class="productTitle">{{product.title}}</h2>
        <img :src="product.image" :alt="product.title">
        <h2 class="productPrice"><TheProductPrice :value="Number(product.price)"/></h2>
        <p>{{product.description.slice(0, 200)}}</p>
        <button class="btn-add" @click="$emit('addtoCart', product)">Add to Cart</button>
        <hr>
      </div>
    </div>
</template>

<script>
import TheProductPrice from '@/components/TheProductPrice.vue';
    export default {
        name: 'TheProductCard',
        emits: ['addtoCart'],
        components: {
            TheProductPrice,
        },
        props: {
            filteredProductsByPrice: Object,
            priceRange: Number,
            

        }

    }
</script>

<style scoped>

</style>