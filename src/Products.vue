<script setup>
import { ref, computed, onMounted } from 'vue';
import ProductsFilter from "@/ProductsFilter.vue";
import Rating from "@/Rating.vue";

const products = ref([]);
const productsToShow = ref([]);
const productFilter = ref(null);

const productsLength = computed(() => {
  return products.value.length;
});

const cheapestPrice = computed(() => {
  return Math.min(...products.value.map(product => product.unit_price));
});

onMounted(() => {
  fetch('/data/products.json')
      .then((response) => response.json())
      .then((data) => {
        products.value = data;
        productsToShow.value = data;
      })
      .catch((error) => console.log('error fetch products dans products.vue', error));
});
</script>

<template>
  <div>
    <h1 class="text-xl p-4 text-center uppercase mb-4 bg-sky-300 text-sky-800">
      Nos produits<br>
      Nombre de produits: {{ productsLength }}
    </h1>

    <ProductsFilter ref="productFilter" v-if="productsLength > 0" v-model:products="productsToShow"></ProductsFilter>

    <div class="container mx-auto flex flex-wrap" v-if="products.length > 0">
      <Product v-for="product in productsToShow" :key="product.id" :data="product" :cheapest-price="cheapestPrice"></Product>
    </div>
    <div v-else>
      <p>Il n'y a pas encore de produits !</p>
    </div>
    <div>
      <button @click="productFilter.reset()" class="rounded bg-indigo-600 px-2 py-1 text-xs font-semibold text-white shadow-sm">Reset</button>
    </div>
  </div>
</template>

<style scoped>

</style>
