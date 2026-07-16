<script setup>
import EncabezadoApp from './components/EncabezadoApp.vue'
import FormularioProveedor from './components/FormularioProveedor.vue'
</script>

<template>
  <div class="app-contenedor">
    <EncabezadoApp
      titulo="Registro de Proveedores"
      subtitulo="Sistema de apoyo para la gestión comercial"
    />

    <main class="main">
      <FormularioProveedor />
    </main>
  </div>
</template>

<style>
/* Estilos globales para todo el cuerpo del examen */
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f3f4f6;
  color: #111827;
}

.app-contenedor {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
}

.main {
  margin-top: 20px;
}
</style>