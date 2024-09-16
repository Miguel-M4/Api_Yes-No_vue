<template>
  <div class="text-center">
    <p class="text-lg font-semibold text-black">{{ answer }}</p>
    <img v-if="image" :src="image" alt="Respuesta de la API" class="mt-4 max-w-xs mx-auto" />
  </div>
</template>

<script>
export default {
  props: {
    answer: {
      type: String,
      required: true
    },
    image: {
      type: String,
      required: false
    }
  }
};
</script>

