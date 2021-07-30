<template>
  <div class="home">
    <nav class="my-3">
      <div>
        <h1>MY SHOP</h1>
      </div>
      <div>
      <TheCart
      :cartProducts="cartProducts"
      :cartStatus="cartStatus"
      :cartTotal="cartTotal"
      :cartQty="cartQty"
      @toggleCart="toggleCart"
      @removeCart="removeCart"
      />
      </div>
    </nav>
    <TheFilter
      :textString="filterText"
      :rangestatus="rangestatus"
      v-model="priceRange"
      @toggle-filter="toggleFilter"
    />
    <TheProductCard
      :filteredProductsByPrice="filteredProductsByPrice"
      :priceRange="priceRange"
      @addtoCart="addCart"
    />

  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
import TheCart from "@/components/TheCart.vue";
import TheFilter from "@/components/TheFilter.vue";
import TheProductCard from "@/components/TheProductCard.vue";
export default {
  name: "Home",
  components: {
    TheCart,
    TheFilter,
    TheProductCard,
  },
  data() {
    return {
      filterText: "Filter Items by Price ",
      rangestatus: false,
      priceRange: 1000,
      products: [],
      cartProducts: [],
      cartStatus: false,
    };
  },
  computed: {
    cartTotal() {
      let sum = 0;
      this.cartProducts.forEach((element) => {
        sum += element.product.price * element.qty;
      });
      return sum;
    },
    cartQty() {
      return this.cartProducts.reduce(
        (total, curr) => (total = total + curr.qty),
        0
      );
    },
    filteredProductsByPrice() {
      return this.products
        .filter((product) =>
          product.price <= this.priceRange && product.price > 0
            ? product.price
            : ""
        )
        .sort(function (a, b) {
          return b.price - a.price;
        });
    },
  },
  methods: {
    addCart(product) {
      var whichProduct;
      var existing = this.cartProducts.filter(function (item, index) {
        if (item.product.id == Number(product.id)) {
          whichProduct = index;
          return true;
        } else {
          return false;
        }
      });
      if (existing.length) {
        this.cartProducts[whichProduct].qty++;
      } else {
        this.cartProducts.push({ product: product, qty: 1 });
      }
    },
    removeCart(e) {
      if (this.cartProducts[e].qty > 1) {
        this.cartProducts[e].qty--;
      } else {
        this.cartProducts.splice(e, 1);
      }
      if (this.cartProducts.length == 0) {
        this.cartStatus = false;
      }
    },
    toggleFilter() {
      this.rangestatus = !this.rangestatus;
    },
    toggleCart() {
      this.cartStatus = !this.cartStatus;
    },
  },
  created() {
    axios
      .get("https://fakestoreapi.com/products/")
      .then((response) => {
        this.products = response.data;
      })
      .catch((err) => {
        console.log(err);
      });
  },
};
</script>
<style>
/* .mainContent {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 10px;
} */
/* .home {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-gap: 10px;
}
.home > * {
  grid-column: 1 / span 12;
} */
/* .content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
} */
/* .card {
  padding: 10px;
} */
/* .content p {
  max-width: 300px;
} */
/* nav {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}
.h3div > * {
  margin: 10px;
} */
/* .h3div {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  margin: auto;
} */
.btn-filter {
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
  border: 3px solid #f7fff7;
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
.cart-wrapper {
position: fixed;
z-index: 1;
}
.cartDiv {
  position: relative;
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
  border: 3px solid #f7fff7;
  border-radius: 10px;
  min-width: 150px;
  min-height: 30px;
  padding: 5px;
}
.btn-cart:hover {
  background: #f7fff7;
  color: #0077b6;
  border: 3px solid #0077b6;
  padding: 5px;
}
.dropDown {
  background-color: #f7fff7;
  border-radius: 10px;
  white-space: wrap;
  overflow: hidden;
  margin: auto;
}
.dropDown-wrapper {
  display: flex;
  flex-direction: column;
  width: 400px;
  border: 3px solid #0077b6;
  border-radius: 10px;
  text-align: start;
  position: absolute;
}
.colorBadge {
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
.remove-btn {
  min-width: 25px;
  cursor: pointer;
  border-radius: 4px;
  border: none;
  background-color: #e63946;
  color: #f7fff7;
  font-weight: bolder;
  margin: 5px 10px;
}
.remove-btn:hover {
  background-color: #962730;
}
</style>
