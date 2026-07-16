<script setup>
import { ref } from 'vue'

// Campos reactivos para vincularlos con v-model en el formulario.
const empresa = ref('')
const contacto = ref('')
const telefono = ref('')
const correo = ref('')
const categoria = ref('')
const ciudad = ref('')

// Estados reactivos para la retroalimentación visual de las validaciones
const mensajeError = ref('')
const mensajeExito = ref('')

// Arreglo reactivo para almacenar temporalmente los proveedores en memoria
const proveedores = ref([])

// Función para registrar un nuevo proveedor
const registrar = () => {
  // Limpiamos mensajes previos
  mensajeError.value = ''
  mensajeExito.value = ''

  // Validación: campos obligatorios no deben estar vacíos
  if (!empresa.value || !contacto.value || !telefono.value || !correo.value || !categoria.value) {
    mensajeError.value = 'Complete todos los campos obligatorios.'
    return
  }

  // Agregamos el proveedor al arreglo en memoria
  proveedores.value.push({
    empresa: empresa.value,
    contacto: contacto.value,
    telefono: telefono.value,
    correo: correo.value,
    categoria: categoria.value,
    ciudad: ciudad.value
  })

  // Mensaje de éxito
  mensajeExito.value = 'Proveedor registrado correctamente.'

  // Limpieza de campos (Requisito de la consigna)
  empresa.value = ''
  contacto.value = ''
  telefono.value = ''
  correo.value = ''
  categoria.value = ''
  ciudad.value = ''

  // Desvanecer el mensaje de éxito después de unos segundos
  setTimeout(() => {
    mensajeExito.value = ''
  }, 4000)
}
</script>

<template>
  <div class="seccion-proveedores">
    <div class="formulario-card">
      <h2>Nuevo Proveedor</h2>
      
      <form @submit.prevent="registrar">
        <div class="grupo-campo">
          <label>Nombre de la empresa *</label>
          <input type="text" v-model.trim="empresa" placeholder="Ej. Distribuidora Santa Cruz" />
        </div>

        <div class="grupo-campo">
          <label>Nombre del contacto *</label>
          <input type="text" v-model.trim="contacto" placeholder="Ej. Juan Pérez" />
        </div>

        <div class="fila-doble">
          <div class="grupo-campo">
            <label>Teléfono *</label>
            <input type="text" v-model.trim="telefono" placeholder="Ej. 76543210" />
          </div>

          <div class="grupo-campo">
            <label>Correo electrónico *</label>
            <input type="email" v-model.trim="correo" placeholder="Ej. contacto@empresa.com" />
          </div>
        </div>

        <div class="fila-doble">
          <div class="grupo-campo">
            <label>Categoría *</label>
            <select v-model="categoria">
              <option value="" disabled>Seleccione una categoría</option>
              <option value="Alimentos">Alimentos</option>
              <option value="Tecnología">Tecnología</option>
              <option value="Ropa">Ropa</option>
              <option value="Servicios">Servicios</option>
              <option value="Otros">Otros</option>
            </select>
          </div>

          <div class="grupo-campo">
            <label>Ciudad</label>
            <input type="text" v-model.trim="ciudad" placeholder="Ej. Santa Cruz" />
          </div>
        </div>

        <div v-if="mensajeError" class="alert error">
          {{ mensajeError }}
        </div>
        <div v-if="mensajeExito" class="alert exito">
          {{ mensajeExito }}
        </div>

        <button type="submit" class="btn-registro">Registrar proveedor</button>
      </form>
    </div>

    <div class="tabla-card">
      <h2>Proveedores Registrados</h2>

      <div v-if="proveedores.length === 0" class="sin-datos">
        <p>No existen proveedores registrados.</p>
      </div>

      <div v-else class="tabla-contenedor">
        <table>
          <thead>
            <tr>
              <th>Empresa</th>
              <th>Contacto</th>
              <th>Teléfono</th>
              <th>Correo</th>
              <th>Categoría</th>
              <th>Ciudad</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(prov, index) in proveedores" :key="index">
              <td><strong>{{ prov.empresa }}</strong></td>
              <td>{{ prov.contacto }}</td>
              <td>{{ prov.telefono }}</td>
              <td>{{ prov.correo }}</td>
              <td><span class="categoria-badge">{{ prov.categoria }}</span></td>
              <td>{{ prov.ciudad || '-' }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<style scoped>
.seccion-proveedores {
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.formulario-card, .tabla-card {
  background: white;
  padding: 24px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  border: 1px solid #e5e7eb;
}

h2 {
  margin-top: 0;
  margin-bottom: 20px;
  font-size: 1.25rem;
  color: #1f2937;
  border-bottom: 2px solid #3b82f6;
  padding-bottom: 8px;
}

.grupo-campo {
  margin-bottom: 16px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

label {
  font-size: 0.875rem;
  font-weight: 600;
  color: #4b5563;
}

input, select {
  padding: 10px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  font-size: 0.95rem;
  outline: none;
}

input:focus, select:focus {
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.15);
}

.fila-doble {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}

@media (min-width: 600px) {
  .fila-doble {
    grid-template-columns: 1fr 1fr;
  }
}

.btn-registro {
  width: 100%;
  background-color: #3b82f6;
  color: white;
  border: none;
  padding: 12px;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s;
  margin-top: 10px;
}

.btn-registro:hover {
  background-color: #2563eb;
}

.alert {
  padding: 12px;
  border-radius: 6px;
  font-size: 0.9rem;
  margin-bottom: 15px;
  font-weight: 500;
}

.alert.error {
  background-color: #fee2e2;
  color: #dc2626;
  border: 1px solid #fca5a5;
}

.alert.exito {
  background-color: #d1fae5;
  color: #065f46;
  border: 1px solid #6ee7b7;
}

.sin-datos {
  text-align: center;
  padding: 40px 20px;
  color: #6b7280;
  border: 2px dashed #e5e7eb;
  border-radius: 6px;
}

.tabla-contenedor {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
  font-size: 0.9rem;
}

th {
  background-color: #f3f4f6;
  color: #374151;
  padding: 12px;
  font-weight: 600;
  border-bottom: 2px solid #e5e7eb;
}

td {
  padding: 12px;
  border-bottom: 1px solid #e5e7eb;
  color: #4b5563;
}

.categoria-badge {
  background-color: #eff6ff;
  color: #1e40af;
  padding: 4px 8px;
  border-radius: 9999px;
  font-size: 0.8rem;
  font-weight: 500;
}
</style>
