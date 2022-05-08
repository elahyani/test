<template>
  <div class="flex flex-col justify-center items-center w-full">
    <div class="m-16">
      <label for="sortTypes">Sort By: </label>
      <select
        name="sortType"
        id="sortTypes"
        v-model="sortType"
        class="border p-1 border-orange-300 rounded-md m-2 outline-orange-500"
      >
        <option value="default" selected disabled>default</option>
        <option value="lowest">Lowest price</option>
        <option value="highest">Highest price</option>
      </select>
    </div>
    <ul class="flex flex-wrap justify-center w-5/6 m-2">
      <li v-for="product in sortedProducts" :key="product.title">
        <SingleProduct :product="product" />
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import SingleProduct from "@/components/SingleProduct.vue";

export default {
  components: {
    SingleProduct,
  },
  data() {
    return {
      products: [],
      sortType: "default",
    };
  },
  created() {
    axios("https://fakestoreapi.com/products")
      .then((response) => {
        this.products = response.data;
      })
      .catch((err) => {
        console.log(err);
      });
  },
  computed: {
    sortedProducts() {
      if (this.sortType === "lowest") {
        return [...this.products].sort((a, b) => a.price - b.price);
      } else if (this.sortType === "highest") {
        return [...this.products].sort((a, b) => b.price - a.price);
      }
      return this.products;
    },
  },
};
</script>
