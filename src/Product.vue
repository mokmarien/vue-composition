<script setup>
import Rating from './Rating.vue'

defineProps({
  product: {
    type: Object,
    required: true
  },
  cheapestPrice: {
    type: Number,
    default: 0
  },
  cheapestPriceColor: {
    type: String,
    default: 'bg-green-500'
  }
})
</script>

<template>
  <div class="mx-2 my-4 rounded shadow" :class="[product.unit_price === cheapestPrice ? cheapestPriceColor : 'bg-white']">
    <h3 class="text-xl text-center bg-sky-200 text-sky-500">{{ product.name }}</h3>
    <img :src="'https://picsum.photos/140/100?random='+product.id" class="w-full card-img-top" alt="..."/>
    <div class="py-2 px-4">
      <p class="text-sm">
        <b>Prix unitaire:</b> {{ product.unit_price }} €<br>
        <b>Quantité:</b> {{ product.quantity }} en stock
      </p>
      <div class="text-sm" v-html="product.description">
      </div>
    </div>
       <Rating :rating="product.rating" :max-rating=6>
            <template #plain>
              <i class="fa-solid fa-face-grin-stars"></i>
            </template>
            <template #half>
              <i class="fa-regular fa-star-half"></i>
            </template>
            <template #empty>
              <i class="fa-regular fa-star"></i>
              </template>
          </Rating>
  </div>
</template>

<style scoped>

</style>
