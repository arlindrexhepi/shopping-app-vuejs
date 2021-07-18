<template>
  <div class="home">
    <nav>
      <h4>Shopping App</h4>
      <div class="cart-wrapper">
        <div class="cartDiv">
          <button class="btn-cart" @click="toggleCart"><span class="colorBadge"> {{'$ '+parseFloat(cartTotal).toFixed(2)}} </span><span> Cart<sup><span>{{cartQty}}</span></sup> </span><span>&#x21e9;</span></button>
        </div>
        <div class="dropDown-wrapper" v-if="cartProducts.length > 0 && cartStatus">
        <div class="dropDown" v-for="(cartProduct, index) in cartProducts" :key="index">
          <div>{{cartProduct.qty}} - {{cartProduct.product.title}} - {{'$'+parseFloat(cartProduct.product.price).toFixed(2)}} -<strong>{{'$'+parseFloat(cartProduct.qty * cartProduct.product.price).toFixed(2)}}</strong><button @click="removeCart(index)">&minus;</button></div>
        </div>
        </div>
      </div>
    </nav>
    <div class="h3div">
      <button class="btn-filter" @click="toggleFilter">{{filterText}} <strong>&dollar;</strong></button>
      <div v-if="rangestatus">
      <input type="text" v-model="priceRange">
      <input type="range" min="0" max="1000" v-model="priceRange">
      </div>
    </div>
      <div class="content card" v-for="(product, index) in filteredProductsByPrice" :key="index">
        <template v-if="product.price<=Number(priceRange)">
        <h2 class="productTitle">{{product.title}}</h2>
        <img :src="product.image" :alt="product.title">
        <!-- <h2 class="productPrice">{{computedProductPrice()}}</h2> -->
        <h2 class="productPrice">{{String('$')+parseFloat(product.price).toFixed(2)}}</h2>
        <p>{{product.description.slice(0, 200)}}</p>
        <button class="btn-add" @click="addCart(product)">Add to Cart</button>
        <hr>
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
    cartStatus: false
    }
  },
  computed: {
    cartTotal() {
      let sum = 0
      this.cartProducts.forEach(element => {
        sum += (element.product.price * element.qty)
      });
      return sum
    },
    cartQty() {
      return this.cartProducts.reduce((total, curr) => (total=total+curr.qty),0)   
    },
    filteredProductsByPrice() {
      return this.products.filter(product => (product.price < this.priceRange && product.price > 0)? product.price : '').sort(function(a, b){return b.price - a.price})
    },
    //MAKE THE PRICE FILTER
    // computedProductPrice() {
    //   return '$' + parseFloat(this.products.price).toFixed(2)
    // }
  },
  methods: {
    addCart(product) {
      var whichProduct;
      var existing = this.cartProducts.filter(function(item, index) {
        if (item.product.id == Number(product.id)){
          whichProduct = index;
          return true
        } else {
          return false
        }
      })
        if (existing.length) {
          this.cartProducts[whichProduct].qty++
        } else {
          this.cartProducts.unshift({product: product, qty: 1})
        }
      
    },
    removeCart (e) {
       if (this.cartProducts[e].qty > 1) {
         this.cartProducts[e].qty--
       } else {
        this.cartProducts.splice(e,1)
       }
    },
    toggleFilter() {
      this.rangestatus=!this.rangestatus
    },
    toggleCart() {
      this.cartStatus=!this.cartStatus
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
html {
  background-color: #f7fff7;
}
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
.content p {
  max-width: 300px;
}
img {
  max-width: 100px;
}
nav {
  display: flex;
  flex-direction: column;
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
  background: #f95738;
  min-width: 270px;
  min-height: 50px;
  color: #f7fff7;
  font-weight: bolder;
}
.btn-filter:hover {
  background: #f7fff7;
  color: #f95738;
  border: 3px solid #f95738;
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
hr {
  background-color: #f95738;
  margin-top: 25px;
  width: 70%;
  height: 2px;
  border-radius: 10px;
}
.productPrice {
  color: #f95738;
  font-size: 28px;
}
.productTitle {
  color: #0077b6;
}
.cart-wrapper{
  position: fixed;
}
.cartDiv {
  font-weight: bolder;
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  min-width: 200px;
}
.btn-cart {
  display: flex;
  justify-content: space-evenly;
  flex-direction: row;
  align-items: center;
  cursor: pointer;
  font-size: 14px;
  font-weight: bolder;
  color: #f7fff7;
  background: #0077b6;
  border: none;
  border-radius: 10px;
  min-width: 150px;
  min-height: 30px;
  padding: 5px;
}
.btn-cart:hover{
  background: #f7fff7;
  color: #0077b6;
  border: 3px solid #0077b6;
}
.dropDown {
  display: flex;
  justify-content: flex-end;
  background-color: #f7fff7;
  border-radius: 10px;

  
}
.dropDown-wrapper{
  width: 400px;
  border-radius: 10px;
  border: 3px solid #0077b6;
  border-radius: 10px;
}
.colorBadge{
  padding: 5px;
  border-radius: 50%;
  background-color: #f7fff7;
  color: #f95738;
}
.whiteBadge {
  padding: 5px;
  border-radius: 40%;
  background-color: #f7fff7;
  color: #0077b6;
}

</style>
