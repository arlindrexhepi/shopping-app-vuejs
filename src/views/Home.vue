<template>
  <div class="home">
    <nav>
      <h4>Shopping App</h4><h4>Cart<sup>{{cartTotal}}</sup>
      <ul>
        <li v-for="(cartProduct, index) in cartProducts" :key="index"> {{cartProduct.title}}</li>
      </ul>
      </h4>
    </nav>
    <div class="h3div">
      <button class="btn-filter" v-if="!rangestatus" @click="toggleFilter">{{filterText}} <strong>&dollar;</strong></button>
      <template v-else>
      <button class="btn-filter" @click="toggleFilter">{{filterText}} <strong>&dollar;</strong></button>
      <input type="text" v-model="priceRange">
      <input type="range" min="0" max="1000" v-model="priceRange">
      </template>
    </div>
      <div class="content card" v-for="(product, index) in filteredProductsByPrice" :key="index">
        <template v-if="product.price<=Number(priceRange)">
        <h2>{{product.title}}</h2>
        <img :src="product.image" :alt="product.title">
        <h2>{{ String('$') + parseFloat(product.price).toFixed(2)}}</h2>
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
    toggleFilter() {
      this.rangestatus=!this.rangestatus
    }
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
  grid-template-columns: repeat(12, 1fr);
  grid-gap: 10px;
}
.home > * {
grid-column: 1/ span 12;
}
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.card {
  padding: 10px;
}
.card h2 {
  color: #0077b6;
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
.h3div > * {
margin: 10px;
}
.h3div {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  margin:  auto;
}
.btn-filter{
  cursor: pointer;
  font-size: 18px;
  border: none;
  border-radius: 10px;
  background: #0077b6;
  min-width: 270px;
  min-height: 50px;
  color: #f7fff7;
  font-weight: bolder;
}
.btn-filter:hover {
  background: #f7fff7;
  color: #0077b6;
  border: 3px solid #0077b6;
}
.btn-add {
  cursor: pointer;
  font-size: 14px;
  border: none;
  border-radius: 10px;
  background: #0077b6;
  min-width: 170px;
  min-height: 50px;
  font-weight: bolder;
  color: #f7fff7;
}
.btn-add:hover {
  background: #f7fff7;
  color: #0077b6;
  border: 3px solid #0077b6;
}
</style>
