<script setup>
import { computed } from 'vue'
const props = defineProps({
  alerta: {
    type: Object,
    required: true
  }
})
const isError = computed(() => {
  return props.alerta.tipo === 'error'
})
</script>

<template>
  <div
    class="text-white text-center p-3 font-bold mb-3 rounded-lg absolute right-0 left-0 top-3 max-w-[90%] w-96 mx-auto"
    :class="isError ? 'bg-red-500' : 'bg-lime-600'"
  >
    {{  alerta.mensaje }}
  </div>
</template>