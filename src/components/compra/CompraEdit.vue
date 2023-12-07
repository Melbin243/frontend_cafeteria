<script setup lang="ts">
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'

const props = defineProps<{
  ENDPOINT_API: string
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
const total = ref('')
const idUsuario = ref('')
const id = router.currentRoute.value.params['id']

async function editarCompra() {
  await http
    .patch(`${ENDPOINT}/${id}`, {
      total: total.value,
      idUsuario: idUsuario.value,
    })
    .then(() => router.push('/compras'))
}

async function getCompra() {
  await http.get(`${ENDPOINT}/${id}`).then((response) => {
    ;(total.value = response.data.total),
      (idUsuario.value = response.data.idUsuario)
  })
}

function goBack() {
  router.go(-1)
}

onMounted(() => {
  getCompra()
})
</script>

<template>
  <div class="container" style="background-color: black;">
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <li class="breadcrumb-item">
          <RouterLink to="/compras">Compras</RouterLink>
        </li>
        <li class="breadcrumb-item active" aria-current="page">Editar</li>
      </ol>
    </nav>

    <div class="row">
      <h2 style="color: white">Editar Compra</h2>
    </div>

    <div class="row">
      <form @submit.prevent="editarCompra">
        <div class="form-floating mb-3">
          <input
            type="string"
            class="form-control"
            v-model="total"
            placeholder="Total"
            required
          />
          <label for="total">Total</label>
        </div>
        <div class="form-floating mb-3">
          <input
            type="string"
            class="form-control"
            v-model="idUsuario"
            placeholder="idUsuario"
            required
          />
          <label for="idUsuario">idUsuario</label>
        </div>
        <div class="text-center mt-3">
          <button type="submit" class="btn btn-primary btn-lg">Editar</button>
        </div>
      </form>
    </div>
    <div class="text-left">
      <button class="btn btn-link" @click="goBack">Volver</button>
    </div>
  </div>
</template>

<style></style>
