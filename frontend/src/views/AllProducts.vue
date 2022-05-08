<template>
  <div class="flex flex-col justify-center items-center w-full">
    <!-- //? the products handlers [sort by price, filter by category] -->
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

      <label for="categories" class="ml-8">Filter By Categories:</label>
      <select
        name="filterByCategory"
        id="categories"
        v-model="selectedCategory"
        class="border p-1 border-orange-300 rounded-md m-2 outline-orange-500"
      >
        <option v-for="category in categories" :key="category">
          {{ category }}
        </option>
      </select>
    </div>
    <!-- //? List all products -->
    <ul class="flex flex-wrap justify-center w-5/6 m-2">
      <li v-for="product in handledProducts" :key="product.title">
        <!-- //? Singe product compenent -->
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
      selectedCategory: "all",
      categories: ["all"],
    };
  },
  created() {
    //? get products from the products API
    axios("https://fakestoreapi.com/products")
      .then((response) => {
        this.products = response.data;
        this.categories = [
          ...this.categories,
          ...new Set([...response.data].map((product) => product.category)),
        ];
      })
      .catch((err) => {
        console.log(err.message);
      });
  },
  computed: {
    //? handle the product sort and filter
    handledProducts() {
      const tmpProducts = [...this.products];
      if (this.sortType === "lowest") {
        tmpProducts.sort((a, b) => a.price - b.price);
      } else if (this.sortType === "highest") {
        tmpProducts.sort((a, b) => b.price - a.price);
      }
      return this.selectedCategory === "all"
        ? tmpProducts
        : tmpProducts.filter(
            (product) => product.category === this.selectedCategory
          );
    },
  },
};
</script>
