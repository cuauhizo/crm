<script lang="ts" setup>
import { FormKit } from "@formkit/vue";
import { useRouter } from 'vue-router';
// import axios from '../lib/axios';
import ClienteService from "../services/ClienteService";
import Heading from "@/components/UI/Heading.vue";
import RouterLink from "@/components/UI/RouterLink.vue";

const router = useRouter()

// console.log(router);

const handleSubmit = (data) => {
  data.estado = 1
  // axios.post('/clientes', data)
  ClienteService.agregarCliente(data)
  .then(respuesta => {
    console.log(respuesta)
    //Redireccionar
    router.push('/')
  })
  .catch(error => console.log(error))
}

</script>
<template>
  <Heading>Agregar Cliente</Heading>
  <div class="flex justify-end">
    <RouterLink to="inicio">Volver</RouterLink>
  </div>
  <div class="mx-auto mt-10 bg-white shadow">
    <div class="mx-auto md:w-2/3 py-20 px-6">
      <FormKit type="form"
        submit-label="Agregar Cliente"
        incomplete-message="No se pudo enviar, revisa los mensajes"
        @submit="handleSubmit">
        <FormKit
          type="text"
          label="Nombre"
          name="nombre"
          placeholder="Nombre de cliente"
          validation="required"
          :validation-messages="{
            required: 'El nombre del Cliente es Obligatorio',
          }"
        />
        <FormKit
          type="text"
          label="Apellido"
          name="apellido"
          placeholder="Apellido de cliente"
          validation="required"
          :validation-messages="{
            required: 'El Apellido del Cliente es Obligatorio',
          }"
        />
        <FormKit
          type="email"
          label="Email"
          name="email"
          placeholder="Email de cliente"
          validation="required|email"
          :validation-messages="{
            required: 'El Email del Cliente es Obligatorio',
            email: 'Ingresa un email válido'
          }"
        />
        <FormKit
          type="text"
          label="Teléfono"
          name="telefono"
          placeholder="Teléfono: XXX-XXX-XXXX"
          validation="?matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
          :validation-messages="{maches:'El formato no es válido'}"
        />
        <FormKit
          type="text"
          label="Empresa"
          name="empresa"
          placeholder="Empresa de cliente"
        />
        <FormKit
          type="text"
          label="Puesto"
          name="puesto"
          placeholder="Puesto de cliente"
        />
      </FormKit>
    </div>
  </div>
</template>
<style>
.formkit-wrapper {
    max-width: 100%;
}
</style>