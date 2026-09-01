<script>
import { ref, computed , onMounted, watch} from 'vue'
export default {
  name: 'ProductsFilter',
  props: {
    products: {
      type: Array,
      required: true
    }
  },
  emits: ['update:products'],
  setup(props, { emit }) {
    const inStock = ref(localStorage.getItem('inStock') === 'true' ? true : false)
    const orderBy = ref(localStorage.getItem('orderBy') || null)
    const search = ref('')
    const originalProducts = ref([])

    onMounted(() => {
      originalProducts.value = [...props.products]
    })

    const manageChange = () => {
      emit('update:products', productsFilteredAndOrdered.value);
    }

    const productsFilteredAndOrdered = computed(() => {
      // Copie des produits reçus
      let products = [...props.products]

      // Filtre stock
      if (inStock.value) {
        products = products.filter(product => product.quantity > 0)
      }

      // Recherche par nom
      const searchLower = search.value.toLowerCase().trim()

      if (searchLower) {
        products = products.filter(product =>
          product.name.toLowerCase().includes(searchLower)
        )
      }

      // Tri
      switch (orderBy.value) {
        case 'nameUp':
          products.sort((a, b) =>
            a.name.localeCompare(b.name)
          )
          break

        case 'nameDown':
          products.sort((a, b) =>
            b.name.localeCompare(a.name)
          )
          break

        case 'priceUp':
          products.sort((a, b) =>
            a.unit_price - b.unit_price
          )
          break

        case 'priceDown':
          products.sort((a, b) =>
            b.unit_price - a.unit_price
          )
          break
      }

      return products
    })

    watch(inStock, (newValue) => {
      localStorage.setItem('inStock', newValue)
    })

    watch(orderBy, (newValue) => {
      localStorage.setItem('orderBy', newValue)
    })

    return {
      inStock,
      orderBy,
      search,
      manageChange,
      productsFilteredAndOrdered
    }
  }
}

</script>

<template>
  <aside>
    <h3 class="font-bold text-sky-500 text-xl text-center uppercase">Filtres</h3>
    <form class="p-4 m-2 bg-sky-300" @change="manageChange" >
         <div class="flex justify-center">
        <div class="text-center">
          <label for="search" class="mr-3 text-center font-bold mb-2 text-sky-900">Recherche par nom</label><br>
          <input id="search" type="text" v-model="search"  class="text-sky-700 p-1"/>
        </div>
      </div>
      <div class="flex flex-wrap justify-evenly items-center">
        <div>
          <h4 class="text-center font-bold mb-2 text-sky-900">En stock ?</h4>
          <div class="flex">
            <div class="mx-1">
              <input type="radio" v-model="inStock" name="in_stock" id="inStockFalse" :value="false" />
              <label for="inStockFalse" class="ml-1">Non</label>
            </div>
            <div class="mx-1">
              <input type="radio" v-model="inStock" name="in_stock" id="inStockTrue" :value="true" />
              <label for="inStockTrue" class="ml-1">Oui</label>
            </div>
          </div>
        </div>
        <div>
          <h4 class="text-center font-bold mb-2 text-sky-900">Ordonné par</h4>
          <select v-model="orderBy" >
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