<script setup>
import { onMounted, reactive } from "vue";
import { FormKit } from "@formkit/vue";
import { useRouter, useRoute } from "vue-router";
import ClienteService from "../services/ClienteService";
import Heading from "@/components/UI/Heading.vue";
import RouterLink from "@/components/UI/RouterLink.vue";

const router = useRouter();
const route = useRoute();
const { id } = route.params;
// console.log(router);
console.log(id);

const formData = reactive({
  nombre: "",
  apellido: "",
  email: "",
  telefono: "",
  empresa: "",
  puesto: "",
});

onMounted(() => {
  ClienteService.obtenerCliente(id)
    .then(({ data }) => {
      formData.nombre = data.nombre;
      formData.apellido = data.apellido;
      formData.email = data.email;
      formData.telefono = data.telefono;
      formData.empresa = data.empresa;
      formData.puesto = data.puesto;
    })
    .catch((error) => console.log(error));
});

const handleSubmit = (data) => {
  ClienteService.actualizarCliente(id, data)
  .then(()=> router.push({ name: 'inicio' }))
  .catch(error => console.log(error))
};
</script>
<template>
  <Heading>Editar Cliente</Heading>
  <div class="flex justify-end">
    <RouterLink to="inicio">Volver</RouterLink>
  </div>
  <div class="mx-auto mt-10 bg-white shadow">
    <div class="mx-auto md:w-2/3 py-20 px-6">
      <FormKit
        type="form"
        submit-label="Guardar Cambios"
        incomplete-message="No se pudo enviar, revisa los mensajes"
        @submit="handleSubmit"
        :value="formData"
      >
        <FormKit
          type="text"
          label="Nombre"
          name="nombre"
          placeholder="Nombre de cliente"
          validation="required"
          :validation-messages="{
            required: 'El nombre del Cliente es Obligatorio',
          }"
          v-model="formData.nombre"
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
          v-model="formData.apellido"
        />
        <FormKit
          type="email"
          label="Email"
          name="email"
          placeholder="Email de cliente"
          validation="required|email"
          :validation-messages="{
            required: 'El Email del Cliente es Obligatorio',
            email: 'Ingresa un email válido',
          }"
          v-model="formData.email"
        />
        <FormKit
          type="text"
          label="Teléfono"
          name="telefono"
          placeholder="Teléfono: XXX-XXX-XXXX"
          validation="?matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
          :validation-messages="{ maches: 'El formato no es válido' }"
          v-model="formData.telefono"
        />
        <FormKit
          type="text"
          label="Empresa"
          name="empresa"
          placeholder="Empresa de cliente"
          v-model="formData.empresa"
        />
        <FormKit
          type="text"
          label="Puesto"
          name="puesto"
          placeholder="Puesto de cliente"
          v-model="formData.puesto"
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
