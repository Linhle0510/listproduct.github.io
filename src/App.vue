<template>
  <div class="container-fluid mt-5 mb-5">
    <h2 class="text-center">Welcome to my Shop</h2>
    <div class="d-flex justify-content-center">
  <div v-if ="isLoading" class="spinner-border" role="status">
    <span class="visually-hidden"></span>
  </div>
</div>

    <div class="row g-2">
      <div class="col-md-3">
        <div class="processor p-2">
          <img
            src="https://banner2.cleanpng.com/20180516/vuw/kisspng-shopping-cart-royalty-free-stock-photography-5afbcad19d9a61.1801898915264508976456.jpg"
            width="40"
            alt=""
          />
          <span>{{ count }}</span>
        </div>
        <div class="processor p-2">
          <h6>SEARCH</h6>
          <form class="d-flex header__form">
            <input
              id="input-search"
              class="form-control me-2"
              v-model="key"
              type="search"
              placeholder="Search"
              aria-label="Search"
            />
            <button class="btn btn-outline-success" type="submit">
              Search
            </button>
          </form>
        </div>
        <div class="processor p-2">
          <h6>SORT BY</h6>
          <select class="form-select" @change="sortPrice">
            <option disabled selected>---</option>
            <option value="sort__asc">Increase By Price</option>
            <option value="sort__Desc">Decrease By Price</option>
          </select>
        </div>

        <div class="brand p-2">
          <div
            class="heading d-flex justify-content-between align-items-center"
          >
            <h6 class="text-uppercase">Brand</h6>
            <span>--</span>
          </div>
          <div
            class="d-flex justify-content-between mt-2" v-for="brand in brands"
            :key="brand.id" 
          >
            <div class="form-check">
              <input
                class="form-check-input"
                type="checkbox"
                value=""
                id="flexCheckDefault"
                
              />
              <label class="form-check-label" for="flexCheckDefault" >
                {{ brand.name }}
              </label>
            </div>
            <span>{{ brand.quantity }}</span>
          </div>
        </div>
        <div class="type p-2 mb-2">
          <div
            class="heading d-flex justify-content-between align-items-center"
          >
            <h6 class="text-uppercase">Type</h6>
            <span>--</span>
          </div>
          <div
            class="d-flex justify-content-between mt-2"
            v-for="type in types"
            :key="type.id"
          >
            <div class="form-check">
              <input
                class="form-check-input"
                type="checkbox"
                value=""
                id="flexCheckDefault"
              />
              <label class="form-check-label" for="flexCheckDefault">
                {{ type.name }}
              </label>
            </div>
            <span>{{ type.quantity }}</span>
          </div>
        </div>
      </div>
      
      <div class="col-md-9">
    
        <div class="row g-2">
          <div
            class="col-md-4"
            v-for="product in filteredList()"
            :key="product.id"
          >
            <div class="product py-4">
              <span class="off bg-success">{{ product.sale }}</span>
              <div class="text-center">
                <img :src="product.img" width="200" />
              </div>
              <div class="about text-center">
                <h5>{{ product.name }}</h5>
                <span>{{ product.price }}</span>
              </div>
              <div class="about text-center">
                <h6>{{ product.type }}</h6>
              </div>
              <div
                class="cart-button mt-3 px-2 d-flex justify-content-between align-items-center"
              >
                <button
                  class="btn btn-primary text-uppercase"
                  @click="addToCart()"
                >
                  Add to cart
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      isLoading: true,
      count: 0,
      key:"",
      sorting: -1,
      type: "name",

      brands: [],
      products: [],
      types: [],
      checkedBrands:[],
    };
  },
  methods: {
    addToCart() {
      this.count++;
    },

    filteredList() {
      return this.products.filter((product) =>
        product.name.toLowerCase().includes(this.key.toLowerCase())
      );
    },
    filterByBrands(){
      return this.products.filter((product) =>
        product.brand== this.checkedBrands)
      },

    sortPrice(e) {
      if (e.target.value == "sort__asc") {
        this.products.sort(function(a, b) {
          return a.price - b.price;
        });
      } else {
        this.products.sort(function(a, b) {
          return b.price - a.price;
        });
      }
    },
  },
  async created(){
      const res = await fetch('http://localhost:3000/products');
      const products = await res.json();
      console.log(products);
       this.products = products;
      

      const res1 = await fetch('http://localhost:3000/brands');
      const brands = await res1.json();
      console.log(brands);
       this.brands = brands;

       const res2 = await fetch('http://localhost:3000/types');
      const types = await res2.json();
      console.log(types);
       this.types = types;
      this.isLoading = false;
    }
};
</script>

<style>
body {
  background-color: #eee;
}

.t-products {
  background-image: linear-gradient(
    to right top,
    #5629c0,
    #5625cb,
    #5620d5,
    #551ae0,
    #5412eb
  );
  color: #fff;
  border-radius: 3px;
}

.processor {
  background-color: #fff;
  margin-top: 5px;
  border-bottom: 1px solid #eee;
}

.brand {
  background-color: #fff;
  border-bottom: 1px solid #eee;
}

.type {
  background-color: #fff;
}

.product {
  padding: 10px;
  background-color: #fff;
  border-radius: 5px;
  position: relative;
}

.about span {
  color: #5629c0;
  font-size: 16px;
}

.cart-button button {
  font-size: 12px;
  color: #fff;
  background-color: #5629c0;
  height: 38px;
}

.cart-button button:focus,
button:active {
  font-size: 12px;
  color: #fff;
  background-color: #5629c0;
  box-shadow: none;
}

.product_fav i {
  line-height: 40px;
  color: #5629c0;
  font-size: 15px;
}

.product_fav {
  display: inline-block;
  width: 36px;
  height: 39px;
  background: #ffffff;
  box-shadow: 0px 1px 5px rgba(0, 0, 0, 0.1);
  border-radius: 11%;
  text-align: center;
  cursor: pointer;
  margin-left: 3px;
  -webkit-transition: all 200ms ease;
  -moz-transition: all 200ms ease;
  -ms-transition: all 200ms ease;
  -o-transition: all 200ms ease;
  transition: all 200ms ease;
}

.product_fav:hover {
  background: #5629c0;
}

.product_fav:hover i {
  color: #fff;
}

.about {
  margin-top: 12px;
}

.off {
  position: absolute;
  left: 65%;
  top: 6%;
  width: 80px;
  text-align: center;
  height: 30px;
  line-height: 8px;
  border-radius: 5px;
  font-size: 13px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
}
</style>
