<script>
import { ref, onMounted, onActivated, onDeactivated } from 'vue';

export default {
  props: {
    id: {
      type: Number,
      required: true
    }
  },
  emits: ['close'],
  setup(props, { emit }) {
    const product = ref(null);

    onMounted(() => {
      fetch(`/data/product${props.id}.json`)
          .then((response) => response.json())
          .then((data) => product.value = data)
    });

    onActivated(() => {
      console.log('activated', product.value)
    });

    onDeactivated(() => {
      console.log('deactivated', product.value)
    });

    const closeModal = () => {
      emit('close');
    };

    return {
      product,
      closeModal
    };
  }
}
</script>

<template>

  <div v-if="product" class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-75">
    <div class="bg-white rounded-lg shadow-lg p-6 max-w-lg w-full">
      <div class="flex justify-between items-center border-b pb-3 mb-3">
        <h2 class="text-xl font-semibold">{{ product.name }}</h2>
        <button class="text-gray-600 hover:text-gray-900" @click="closeModal">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
      <p class="text-gray-700 mb-4">{{ product.description }}</p>
      <div class="flex justify-end">
        <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="closeModal">Fermer</button>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
