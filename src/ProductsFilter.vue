<script setup>
import { ref, computed } from 'vue'

const inStock = ref(true)
const orderBy = ref(null)
</script>

<template>
  <aside>
    <h3 class="font-bold text-sky-500 text-xl text-center uppercase">Filtres</h3>
    <form class="p-4 m-2 bg-sky-300">
      <div class="flex flex-wrap justify-evenly items-center">
        <div>
          <h4 class="text-center font-bold mb-2 text-sky-900">En stock ?</h4>
          <div class="flex">
            <div class="mx-1">
              <input type="radio" v-model="inStock" name="in_stock" id="inStockFalse" :value="false"/>
              <label for="inStockFalse" class="ml-1">Non</label>
            </div>
            <div class="mx-1">
              <input type="radio" v-model="inStock" name="in_stock" id="inStockTrue" :value="true"/>
              <label for="inStockFalse" class="ml-1">Oui</label>
            </div>
          </div>
        </div>
        <div>
          <h4 class="text-center font-bold mb-2 text-sky-900">Ordonné par</h4>
          <select v-model="orderBy">
            <option value="nameDown">Nom décroissant</option>
            <option value="nameUp">Nom croissant</option>
            <option value="priceDown">Prix décroissant</option>
            <option value="priceUp">Prix croissant</option>
          </select>
        </div>
      </div>
    </form>
  </aside>
</template>

<style scoped>

</style>


