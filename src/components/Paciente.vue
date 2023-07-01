<script setup>

const props = defineProps({
  paciente: {
    type: Object,
    required: true,
  }
})

defineEmits(['actualizar-paciente', 'eliminar-paciente'])

</script>

<template>
  <div class="p-5 bg-white shadow-md gap-1 rounded-xl grid md:grid-cols-2 text-secondary relative">
    <div class="flex justify-end gap-1 text-white w-full absolute right-5 -top-5">
      <button
        type="button"
        class="p-3 w-10 h-10 flex items-center justify-center rounded-full bg-secondary hover:opacity-90 font-bold uppercase shadow-md"
        @click="$emit('actualizar-paciente', paciente.id)"
      ><i class="fa-solid fa-pencil"></i></button>
      <button
        type="button"
        class="p-3 w-10 h-10 flex items-center justify-center rounded-full bg-red-500 hover:opacity-90 font-bold uppercase shadow-md"
        @click="$emit('eliminar-paciente', paciente.id)"
      ><i class="fa-solid fa-trash-can"></i></button>
    </div>
    <p class="font-bold uppercase">Nombre:
      <span class="font-normal normal-case">
        {{  paciente.nombre }}
      </span>
    </p>
    <p class="font-bold uppercase">Propietario:
      <span class="font-normal normal-case">
        {{  paciente.propietario }}
      </span>
    </p>
    <p class="font-bold uppercase">Email:
      <span class="font-normal normal-case">
        {{  paciente.email }}
      </span>
    </p>
    <p class="font-bold uppercase">Fecha Alta:
      <span class="font-normal normal-case">
        {{  paciente.alta }}
      </span>
    </p>
    <p class="font-bold uppercase col-span-2">Síntomas:
      <span class="font-normal normal-case">
        {{  paciente.sintomas }}
      </span>
    </p>
    <p class="opacity-50 text-xs absolute -right-5 bottom-9 -rotate-90">
      {{ paciente.id }}
    </p>
  </div>
</template>