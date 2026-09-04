<script setup>
import Rating from './Rating.vue'

defineProps({
  data: {
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
});
</script>

<template>
  <div class="mx-2 my-4 rounded shadow" :class="[data.unit_price === cheapestPrice ? cheapestPriceColor : 'bg-white']">
    <h3 class="text-xl text-center bg-sky-200 text-sky-500">{{ data.name }}</h3>
    <img :src="'https://picsum.photos/140/100?random='+data.id" class="w-full card-img-top" alt="..."/>
    <div class="py-2 px-4">
      <p class="text-sm">
        <b>Prix unitaire:</b> {{ data.unit_price }} €<br>
        <b>Quantité:</b> {{ data.quantity }} en stock
      </p>
      <div class="text-sm" v-html="data.description">
      </div>
      <Rating :rating="data.rating"></Rating>
    </div>
  </div>
</template>

<style scoped>

</style>
