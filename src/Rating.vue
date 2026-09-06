<script setup>
import { computed ,inject} from 'vue'



const maxRating = inject('max', '5');
const props = defineProps({
  rating: {
    type: Number,
    required: true
  },
  
})

const numberOfPlainItem = computed(() => {
  return props.rating * 100 % 100 === 0 ? props.rating : Math.floor(props.rating);
})

const numberOfEmptyItem = computed(() => {
  console.log('props.maxRating', maxRating)
   console.log('props.rating', props.rating)
  return maxRating - Math.floor(props.rating);
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
