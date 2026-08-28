<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  products: {
    type: Array,
    required: true
  }
})
const inStock = ref(true)
const orderBy = ref(null)

const orderProductsByName = (is_up) => {
  return props.products.sort((a, b) => {
    const nameA = a.name.toUpperCase();
    const nameB = b.name.toUpperCase();
    if (nameA < nameB) {
      return is_up ? -1 : 1;
    }
    if (nameA > nameB) {
      return is_up ? 1 : -1;
    }
    return 0;
  });
}

const orderProductsByPrice = (is_up) => {
  return props.products.sort((a, b) => {
    const priceA = a.unit_price;
    const priceB = b.unit_price;
    if (priceA < priceB) {
      return is_up ? -1 : 1;
    }
    if (priceA > priceB) {
      return is_up ? 1 : -1;
    }
    return 0;
  });
}

const productsFiltered = computed(() => {
  return inStock.value ? props.products.filter(product => product.quantity > 0) : props.products;
})

const productsFilteredAndOrdered = computed(() => {
  let products = [...productsFiltered.value];

  switch (orderBy.value) {
    case 'nameUp':
      products = orderProductsByName(true)
      break;
    case 'nameDown':
      products = orderProductsByName(false)
      break;
    case 'priceUp':
      products = orderProductsByPrice(true)
      break;
    case 'priceDown':
      products = orderProductsByPrice(false)
      break;
  }

  return products;
})
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


