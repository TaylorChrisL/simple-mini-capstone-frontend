<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to da shop!",
      message2: "Products",
      products: [],
      currentProduct: {},
      formattedPrice: {},
      editProductParams: {},
      newProductParams: {},
      showErrorMessage: false,
      errorMessage: "",
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
    createProduct: function () {
      axios
        .post("http://localhost:3000/products", this.newProductParams)
        .then((response) => {
          console.log("Product created ", response.data);
          this.products.push(response.data);
          this.newProductParams = {};
          this.showErrorMessage = false;
        })
        .catch((error) => (this.errorMessage = error))
        .then((this.showErrorMessage = true));
    },
    showProduct: function (product) {
      console.log(product);
      document.querySelector("#product-details").showModal();
      this.currentProduct = product;
      this.editProductParams = product;
      this.formattedPrice = product.formatted;
    },
    updateProduct: function (product) {
      axios.patch("http://localhost:3000/products/" + product.id, this.editProductParams).then((response) => {
        console.log("We dont it!!!", response.data);
      });
    },
    destroyProduct: function (product) {
      axios.delete("http://localhost:3000/products/" + product.id).then((response) => {
        console.log("Success!", response.data);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>{{ message2 }}</h2>
    <h3>Add New Product</h3>
    <div>
      <label for="name">Name:</label>
      <input type="text" v-model="newProductParams.name" id="name" placeholder="name" />
    </div>
    <div>
      <label for="price">Price:</label>
      <input type="text" v-model="newProductParams.price" id="price" placeholder="price" />
    </div>
    <div>
      <label for="description">Description:</label>
      <input type="text" v-model="newProductParams.description" id="description" placeholder="description" />
    </div>
    <div>
      <label for="image_url">Image Url:</label>
      <input type="text" v-model="newProductParams.image_url" id="image_url" placeholder="image url" />
    </div>
    <div>
      <p v-if="showErrorMessage">{{ errorMessage }}</p>
    </div>
    <div>
      <button v-on:click="createProduct()">Create</button>
    </div>
    <div v-for="product in products" v-bind:key="product.id">
      <img :src="product.image_url" />
      <h5>{{ product.name }}</h5>
      <h6>{{ product.price }}</h6>
      <div>
        <button v-on:click="showProduct(product)">More Info!</button>
      </div>
      <hr />
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <div>
          <label for="editName">Name:</label>
          <input type="text" v-model="editProductParams.name" id="editName" />
        </div>
        <br />
        <div>
          <label for="editDescription">Description:</label>
          <input type="text" v-model="editProductParams.description" id="editDescription" />
        </div>
        <br />
        <div>
          <label for="editPrice">Subtotal:</label>
          <input type="text" v-model="editProductParams.price" id="editPrice" />
        </div>
        <br />
        <div>Tax: {{ formattedPrice.tax }}</div>
        <br />
        <div>Total: {{ formattedPrice.total }}</div>
        <br />
        <div>
          <label for="editImageUrl">Image Url:</label>
          <input type="text" v-model="editProductParams.image_url" id="editImageUrl" />
        </div>
        <button v-on:click="updateProduct(currentProduct)">Update Product</button>
        <button>Close</button>
        <button v-on:click="destroyProduct(currentProduct)">Destroy Product</button>
      </form>
    </dialog>
  </div>
</template>

<style>
h1 {
  color: rgb(87, 7, 216);
}

img {
  height: 300px;
  width: 300px;
}
</style>
