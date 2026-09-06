<script setup>
import { ref, computed, onMounted ,provide} from 'vue';
import ProductsFilter from "@/ProductsFilter.vue";
import Rating from "@/Rating.vue";
import ProductDetailModal from "@/ProductDetailModal.vue";

provide('max', 8);

const products = ref([]);
const productsToShow = ref([]);
const productFilter = ref(null);
const selectedProductId = ref(null);

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

    <div class="container mx-auto">
      <div class="flex flex-wrap" v-if="products.length > 0">
         <TransitionGroup name="list" > 
        <Product v-for="product in productsToShow" :key="product.id" :data="product" :cheapest-price="cheapestPrice"
                 @click="selectedProductId = product.id"></Product>
        </TransitionGroup>

        <template v-for="product in products">
          <KeepAlive>
            <ProductDetailModal v-if="selectedProductId === product.id" :key="product.id" :id="product.id"
                                @close="selectedProductId = null"></ProductDetailModal>
          </KeepAlive>
        </template>
      </div>
      <div v-else>
        <p>Il n'y a pas encore de produits !</p>
      </div>
      <div class="text-center">
        <button @click="productFilter.reset()"
                class="rounded bg-indigo-600 px-2 py-1 text-lg font-semibold text-white shadow-sm">Reset
        </button>
      </div>
    </div>

  </div>
</template>

<style scoped>
.list-move, .list-enter-active, .list-leave-active {
  transition: all 0.5s;
}
.list-enter-from, .list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.list-leave-active {
  position: absolute;
}
</style>
