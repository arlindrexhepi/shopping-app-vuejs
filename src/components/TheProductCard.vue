<template >
<div class="container my-5">
    <div class="row row-cols-1 row-cols-lg-3 g-4">
        <div v-for="(product, index) in filteredProductsByPrice" :key="index" class="col d-flex flex-fill animate__animated animate__fadeInRight">
            <div class="card my-3">
                <div v-if="product.price<=Number(priceRange)">
                    <div class="align-items-center p-2 text-center">
                        <img class="card-img-top rounded" :src="product.image" :alt="product.title">
                        <h2 class="card-title fw-bolder">{{product.title}}</h2>
                        <div class="card-body mt-3 info">
                        <p class="card-text">{{product.description.slice(0, 200)}}</p>
                        </div>
                        <div class="cost mt-3">
                            <h1 class="fw-bolder"><TheProductPrice :value="Number(product.price)"/></h1>
                        </div>
                        <div class="btn-add justify-content-stretch p-3 text-center mt-3 cursor" @click="$emit('addtoCart', product)">
                            <span > {{addItems}} </span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
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
        },
        data() {
            return{
                addItems: 'ADD TO CART',
            }
        }

    }
</script>

<style scoped>
.card-title {
    text-overflow: ellipsis;
}
.card-title{
    color: #0077b6;
}
.card {
    border: 3px solid #0077b6;
    outline: none;
    border-radius: 20px;
}
.card-img-top {
    width: 160px !important;
}
.cost {
    color: #f95738;
}

</style>