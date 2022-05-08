<template>
  <div class="border rounded-md shadow-lg border-orange-300 w-72 py-2 m-2">
    <div class="flex justify-center mt-4">
      <img
        :src="product.image"
        :alt="product.title"
        class="rounded-t-md w-32 h-32"
      />
    </div>
    <div class="relative p-2">
      <h1 class="text-orange-400 font-bold">
        {{ product.title.substring(0, 20) }}...
      </h1>
      <span class="absolute top-2 right-2 font-bold text-orange-600">
        ${{ product.price }}
      </span>
      <p class="font-medium text-sm text-gray-500">
        category: {{ product.category }}
      </p>
      <p class="text-sm text-black text-opacity-50">
        {{ product.description.substring(0, 100).toLowerCase() }}...
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
      required: true,
    },
  },
};
</script>
