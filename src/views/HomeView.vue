<script>
export default {
  data: function () {
    return {
      message: "Welcome to da shop!",
      message2: "Products",
      products: [
        { id: 1, name: "Item1", price: 3.99 },
        { id: 2, name: "Item1", price: 6.99 },
        { id: 1, name: "Item1", price: 2.0 },
      ],
    };
  },
  created: function () {},
  methods: {
    resetMessages: function () {
      this.message2 = "We are here now";
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
    </div>
  </div>
</template>

<style>
h1 {
  color: rgb(87, 7, 216);
}
</style>
