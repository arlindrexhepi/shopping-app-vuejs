<template>
    <div class="cart-wrapper">
      <div class="cartDiv">
        <button class="btn-cart" @click="$emit('toggleCart')"><span class="colorBadge"> <TheProductPrice :value="Number(cartTotal)"/></span><span> Cart<sup><span>{{cartQty}}</span></sup> </span><span>&#x21e9;</span></button>
      </div>
      <div class="dropDown-wrapper" v-if="cartProducts.length > 0 && cartStatus">
        <div class="dropDown" v-for="(cartProduct, index) in cartProducts" :key="index">
          <div>{{cartProduct.qty}} - {{cartProduct.product.title}} - <TheProductPrice :value="Number(cartProduct.product.price)" /> -<strong><TheProductPrice :value="Number((cartProduct.qty * cartProduct.product.price))"/></strong><button class="remove-btn" @click="$emit('removeCart', index)">&minus;</button></div>
        </div>
      </div>
    </div>
</template>

<script>
    import TheProductPrice from '@/components/TheProductPrice.vue'
    export default {
        name: 'TheCart',
        components: {
            TheProductPrice,
        },
        emits: ['toggleCart', 'removeCart'],
        props: {
            cartProducts: Object,
            cartStatus: Boolean,
            cartTotal: Number,
            cartQty: Number,

        }
    }
</script>

<style scoped>

</style>