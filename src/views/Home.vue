<template>
  <div class="home">
    <nav>
      <h4>Shopping App</h4><h4>Cart<sup>{{cartTotal}}</sup></h4>
      <ul>
        <li v-for="(cartProduct, index) in cartProducts" :key="index"> {{cartProduct.title}}</li>
      </ul>
    </nav>
      <span v-if="!rangestatus" @click="rangestatus=!rangestatus" :style="{cursor:cursor}">{{filterText}}</span>
      <span v-else>
      <input type="text" v-model="priceRange">
      <input type="range" min="0" max="1000" v-model="priceRange">
      </span>
      <div class="content" v-for="(product, index) in products" :key="index">
        <template v-if="product.price<=Number(priceRange)">
        <h3>{{product.title}}</h3>
        <img :src="product.image" :alt="product.title">
        <h3>{{product.price | filteredPrice }}</h3>
        <p>{{product.description}}</p>
        <button class="btn-add" >Add to Cart</button>
        </template>
      </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
import func from 'vue-editor-bridge'
export default {
  name: "Home",
  components: {

  },
  data() {
    return {
    filterText: 'Filter Items by Price',
    cursor:'pointer',
    rangestatus:false,
    priceRange: 100,
    products: [],
    cartProducts: [],
    }
  },
  filters: {
    filteredPrice: function(value) {
      String('$') + parseFloat(value).toFixed(2)
    }
  },
  computed: {
    cartTotal () {
      return this.cartProducts.reduce((total, curr) => (total=total+curr.qnt),0)   
    }
  },
  methods: {
    // addCart(e) {
    //   this.cartProducts.unshift({product:this.products[e], qnt: +1})
    //   console.log(this.cartProducts)
    // }
  },
  created() {
    axios.get('https://fakestoreapi.com/products/')
    .then((response) => {
      this.products = response.data
    }).catch((err) => {
      console.log(err);
    });
  }
};
</script>
<style>
img {
  max-width: 70px;
}
nav {
  display: flex;
  justify-content: space-around;
}
</style>
