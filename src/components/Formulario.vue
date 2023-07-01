<script setup>
import { reactive, computed } from 'vue'
import Alert from './Alert.vue'

const alerta = reactive({
  tipo: '',
  mensaje: '',
})

const emits = defineEmits([
  'update:nombre',
  'update:propietario',
  'update:email',
  'update:alta',
  'update:sintomas',
  'guardar-paciente'
])

const props = defineProps({
  nombre: {
    type: String,
    required: true
  },
  propietario: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true
  },
  alta: {
    type: String,
    required: true
  },
  sintomas: {
    type: String,
    required: true
  },
  id: {
    type: [String, null],
    required: true
  }
})

const ocultarAlerta = () => {
  setTimeout(() => {
    Object.assign(alerta, {
      tipo: '',
      mensaje: ''
    })
  }, 3000)
}

const validar = () => {
  if(Object.values(props).includes('')) {
    alerta.tipo = 'error'
    alerta.mensaje = 'Todos los campos son obligatorios'
    ocultarAlerta()
    return
  }
  alerta.tipo = 'success'
  alerta.mensaje = 'Se guardó correctamente'
  emits('guardar-paciente')
  ocultarAlerta()
}

const editando = computed(() => {
  return props.id
})
</script>

<template>
  <Alert
    v-if="alerta.mensaje"
    :alerta="alerta"
  />
  <div class="text-secondary w-full p-5 md:p-10">
    <h2 class="font-black text-3xl">Seguimiento de pacientes</h2>
    <p class="text-md mb-5">
      Añade pacientes y
      <span class="text-primary font-bold">administralos</span>
    </p>
    <form @submit.prevent="validar">
      <div class="mb-3">
        <label for="mascota" class="text-xs opacity-70 block font-bold">Mascota</label>
        <input
          id="mascota"
          type="text"
          placeholder="Nombre de la mascota"
          class="border-2 w-full py-3 px-4  placeholder-gray-400 rounded-lg focus:outline-none focus:border-primary"
          :value="nombre"
          @input="$emit('update:nombre', $event.target.value)"
        >
      </div>
      <div class="mb-3">
        <label for="propietario" class="text-xs opacity-70 block font-bold">Propietario</label>
        <input
          id="propietario"
          type="text"
          placeholder="Nombre del propietario"
          class="border-2 w-full py-3 px-4  placeholder-gray-400 rounded-lg focus:outline-none focus:border-primary"
          :value="propietario"
          @input="$emit('update:propietario', $event.target.value)"
        >
      </div>
      <div class="mb-3">
        <label for="email" class="text-xs opacity-70 block font-bold">Email</label>
        <input
          id="email"
          type="email"
          placeholder="Email"
          class="border-2 w-full py-3 px-4  placeholder-gray-400 rounded-lg focus:outline-none focus:border-primary"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        >
      </div>
      <div class="mb-3">
        <label for="alta" class="text-xs opacity-70 block font-bold">Fecha de Alta</label>
        <input
          id="alta"
          type="date"
          class="border-2 w-full py-3 px-4  placeholder-gray-400 rounded-lg focus:outline-none focus:border-primary"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
        >
      </div>
      <div class="mb-3">
        <label for="sintomas" class="text-xs opacity-70 block font-bold">Síntomas</label>
        <textarea
          id="sintomas"
          placeholder="Describe los sintomas"
          class="border-2 w-full py-3 px-4  placeholder-gray-400 rounded-lg focus:outline-none focus:border-primary"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
        />
      </div>
      <button
        type="submit"
        class="bg-secondary w-full p-3 flex items-center gap-3 justify-center
        text-white uppercase font-bold hover:opacity-90
        cursor-pointer transition-colors rounded-lg shadow-md
        "
      >
        {{ editando ? 'Editar' : 'Registrar' }}
        <i class="fa-solid fa-dog"></i>
      </button>
    </form>
  </div>
</template>