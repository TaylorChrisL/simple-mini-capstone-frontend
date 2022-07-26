<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to da shop!",
      message2: "Products",
      products: [],
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    resetMessages: function () {
      this.message2 = "We are here now";
    },
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
        console.log("All Products: ", this.products);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>{{ message2 }}</h2>
    <div v-for="product in products" v-bind:key="product.id">
      <h5>{{ product.name }}</h5>
      <h6>{{ product.price }}</h6>
      <img :src="product.image_url" />
    </div>
  </div>
</template>

<style>
h1 {
  color: rgb(87, 7, 216);
}
</style>
