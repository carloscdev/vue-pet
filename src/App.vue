<script setup>
import { ref, reactive, watch, onMounted } from 'vue'
import { uid } from 'uid'
import Empty from './components/Empty.vue'
import Formulario from './components/Formulario.vue'
import Paciente from './components/Paciente.vue'
import Header from './components/Header.vue'

const pacientes = ref([])
const paciente = reactive({
  id: null,
  nombre: '',
  propietario: '',
  email: '',
  alta: '',
  sintomas: ''
})

watch(pacientes, () => {
  guardarLocalStorage()
}, { deep: true })

const guardarPaciente = () => {
  if (paciente.id) {
    const { id } = paciente
    const i = pacientes.value.findIndex(item => item.id === id)
    pacientes.value[i] = {...paciente}
  } else {
    pacientes.value.push({
      ...paciente,
      id: uid()
    })
  }
  // Reiniciar el objeto usando Object.assign
  Object.assign(paciente, {
    nombre: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: '',
    id: null
  })
}

const actualizarPaciente = (id) => {
  const pacienteEditar = pacientes.value.filter((item) => item.id === id)[0]
  Object.assign(paciente, pacienteEditar)
}

const eliminarPaciente = (id) => {
  pacientes.value = pacientes.value.filter((item) => item.id !== id)
}

const guardarLocalStorage = () => {
  localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
}

onMounted(() => {
  const pacientesStorage = localStorage.getItem('pacientes')
  if (pacientesStorage) {
    pacientes.value = JSON.parse(pacientesStorage)
  }
})
</script>

<template>
  <div class="grid md:grid-cols-2 relative">
    <Header />
    <section class="h-screen bg-white flex items-center justify-center">
      <Formulario
        v-model:nombre="paciente.nombre"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
        @guardar-paciente="guardarPaciente"
        :id="paciente.id"
      />
    </section>
    <section class="h-screen flex items-center justify-center text-white">
      <div v-if="pacientes.length > 0" class="w-full p-5 md:p-10">
        <h3 class="font-black text-3xl">
          Administra tus pacientes
        </h3>
        <p class="text-lg">
          Información de
          <span class="text-secondary font-bold">pacientes</span>
        </p>
        <div class="flex flex-col gap-8 md:h-[490px] overflow-y-auto pt-5">
          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>
      </div>
      <Empty v-else>No hay pacientes</Empty>
    </section>
  </div>
</template>
