<template>
  <div class="home">
    <nav>
      <h4>Shopping App</h4><h4>Cart<sup>{{cartTotal}}</sup>
      <ul>
        <li v-for="(cartProduct, index) in cartProducts" :key="index"> {{cartProduct.title}}</li>
      </ul>
      </h4>
    </nav>
      <h3 v-if="!rangestatus" @click="rangestatus=!rangestatus" :style="{cursor:cursor}">{{filterText}} <strong>&dollar;</strong></h3>
      <span v-else>
      <h3 @click="rangestatus=!rangestatus" :style="{cursor:cursor}">{{filterText}} <strong>&dollar;</strong></h3>
      <input type="text" v-model="priceRange">
      <input type="range" min="0" max="1000" v-model="priceRange">
      </span>
      <div class="content" v-for="(product, index) in filteredProductsByPrice" :key="index">
        <template v-if="product.price<=Number(priceRange)">
        <h3>{{product.title}}</h3>
        <img :src="product.image" :alt="product.title">
        <h3>{{ String('$') + parseFloat(product.price).toFixed(2) }}</h3>
        <p>{{product.description.slice(0, 200)}}</p>
        <button class="btn-add" >Add to Cart</button>
        </template>
      </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
export default {
  name: "Home",
  components: {

  },
  data() {
    return {
    filterText: 'Filter Items by Price ',
    cursor:'pointer',
    rangestatus:false,
    priceRange: 100,
    products: [],
    cartProducts: [],
    }
  },
  computed: {
    cartTotal() {
      return this.cartProducts.reduce((total, curr) => (total=total+curr.qnt),0)   
    },
    filteredProductsByPrice() {
      return this.products.filter(product => (product.price < this.priceRange && product.price > 0)? product.price : product.price).sort(function(a, b){return b.price - a.price})
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
.home {
  display: grid;
  grid-template-columns: 1fr;
}
.content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.content p {
  max-width: 300px;
}
img {
  max-width: 100px;
}
nav {
  display: flex;
  justify-content: space-around;
}
</style>
