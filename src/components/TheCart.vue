<template>
    <div class="cart-wrapper animate__animated animate__fadeInDownBig">
      <div class="cartDiv">
        <button class="btn-cart" @click="$emit('toggleCart')"><span class="colorBadge"> <TheProductPrice :value="Number(cartTotal)"/></span><font-awesome-icon icon="shopping-cart" :style="{fontSize: fontSize + 'px'}"></font-awesome-icon> <sup class="badge rounded-pill bg-danger" :style="{fontSize:'13px'}"><span>{{cartQty}}</span></sup> <span>&#x21e9;</span></button>
      </div>
    <!-- <transition enter-active-class="animate__animated animate__slideInLeft" leave-active-class="animate__animated animate__slideOutLeft" mode="out-in"> -->
      <div class="dropDown-wrapper" v-if="cartProducts.length > 0 && cartStatus">
        <div class="dropDown" v-for="(cartProduct, index) in cartProducts" :key="index">
          <div class="text-end"><span class="badge rounded-pill bg-warning">{{cartProduct.qty}}</span> - {{cartProduct.product.title}} - <strong><TheProductPrice :value="Number(cartProduct.product.price)" /></strong> -<strong :style="{color: '#f95738'}"><TheProductPrice :value="Number((cartProduct.qty * cartProduct.product.price))"/></strong><button class="remove-btn" @click="$emit('removeCart', index)">&minus;</button></div>
        </div>
      </div>
    <!-- </transition> -->
    </div>
</template>

<script>
    import TheProductPrice from '@/components/TheProductPrice.vue';
    import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
    export default {
        name: 'TheCart',
        components: {
            TheProductPrice,
            FontAwesomeIcon,
        },
        emits: ['toggleCart', 'removeCart'],
        props: {
            cartProducts: Object,
            cartStatus: Boolean,
            cartTotal: Number,
            cartQty: Number,

        },
        data () {
          return {
            fontSize: 25,
          }
        }
    }
</script>

<style scoped>
</style>