<script setup>
import { computed } from 'vue'

const props = defineProps({
  rating: {
    type: Number,
    required: true
  },
  maxRating: {
    validator(rating, props) {
      return rating >= props.rating;
    },
    default: 5
  }
})

const numberOfPlainItem = computed(() => {
  return props.rating * 100 % 100 === 0 ? props.rating : Math.floor(props.rating);
})

const numberOfEmptyItem = computed(() => {
  console.log('props.maxRating', props.maxRating)
   console.log('props.rating', props.rating)
  return props.maxRating - Math.floor(props.rating);
})

const numberOfHalfItem = computed(() => {
  return props.rating * 100 % 100 !== 0 ? 1 : 0;
})
</script>

<template>
  <div>
    <slot name="plain" v-for="n in numberOfPlainItem" :key="n">
        <i class="fa-solid fa-star"></i>   
    </slot>

    <slot name="half" v-if="numberOfHalfItem > 0" v-for="n in numberOfHalfItem" :key="n"> 
        <i class="fa-solid fa-star-half-stroke"></i>
    </slot>
   
  <slot name="empty" v-for="n in numberOfEmptyItem" :key="n">
        <i class="fa-regular fa-star"></i>
  </slot>
  </div>
</template>

<style scoped>

</style>
