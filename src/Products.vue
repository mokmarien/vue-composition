<script setup>
import { ref , computed} from 'vue'
import ProductsFilter from './ProductsFilter.vue';

    const products = ref([
     {'id': 1, 'rating': 1.5, 'name': 'Produit A', 'unit_price': 10, 'quantity': 5, 'description': '<b>genial</b>' },
        {'id': 2, 'rating': 2, 'name': 'Produit B', 'unit_price': 140, 'quantity': 4, 'description': '<b style="color: green;">On valide</b>'},
        {'id': 3, 'rating': 3, 'name': 'Produit C', 'unit_price': 15, 'quantity': 5, 'description': null},
        {'id': 4, 'rating': 4, 'name': 'Produit D', 'unit_price': 12, 'quantity': 8, 'description': 'sans commentaire'},
        {'id': 5, 'rating': 5, 'name': 'Produit E', 'unit_price': 4, 'quantity': 5, 'description': '<b>ok</b>'},
        {'id': 6, 'rating': 4, 'name': 'Produit F', 'unit_price': 160, 'quantity': 6, 'description': '<b>cool</b>'},
        {'id': 7, 'rating': 3, 'name': 'Produit G', 'unit_price': 105, 'quantity': 5, 'description': '<b>bof</b>'},
        {'id': 8, 'rating': 2, 'name': 'Produit H', 'unit_price': 50, 'quantity': 8, 'description': '<b>incroyable</b>'},
        {'id': 9, 'rating': 2, 'name': 'Produit I', 'unit_price': 40, 'quantity': 5, 'description': '<b>super</b>'},
        {'id': 10, 'rating': 1, 'name': 'Produit J', 'unit_price': 80, 'quantity': 5, 'description': '<u>génial</u>'}, 
    ]);
  
    const productsLength = computed(() => products.value.length);
    const cheapestPrice = computed(() => Math.min(...products.value.map(product => product.unit_price)));
</script>

<template>
    <div>
 
    <hr>
    <h1 class="text-xl p-4 text-center uppercase mb-4 bg-sky-300 text-sky-800">
      Nos produits <br>
      Nombre de produits: {{ productsLength}}
    </h1>
    <ProductsFilter :products="products"></ProductsFilter>
    <div class="container mx-auto flex flex-wrap" v-if="products.length > 0">

      <Product v-for="product in products" :key="product.id" :product="product" :cheapest-price="cheapestPrice" :cheapest-price-color="'bg-blue-500'">
      </Product>

    </div>
    <div v-else>
      <p>Il n'y a pas encore de produits !</p>
    </div>
  </div>
</template>

<style scoped>

</style>
