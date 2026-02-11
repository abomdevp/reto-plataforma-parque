<script setup>
import { ref } from "vue";

const formulario = ref({
  nombre: "",
  capacidad: null,
  ambiente: "Zona Familiar",
  intensidad: "Suave",
  operativa: false,
  descripcion: "",
});

const atracciones = ref([]);

function guardarAtraccion() {
  if (!formulario.value.nombre.trim()) return;

  const nueva = {
    nombre: formulario.value.nombre,
    capacidad: formulario.value.capacidad,
    ambiente: formulario.value.ambiente,
    intensidad: formulario.value.intensidad,
    operativa: formulario.value.operativa,
    descripcion: formulario.value.descripcion,
  };

  atracciones.value.push(nueva);

  formulario.value = {
    nombre: "",
    capacidad: null,
    ambiente: "Zona Familiar",
    intensidad: "Suave",
    operativa: false,
    descripcion: "",
  };
}

function eliminarAtraccion(index) {
  atracciones.value.splice(index, 1);
}
</script>

<template>
  <div class="container py-4">
    <div class="row g-4">
      <div class="col-12">
        <h1 class="mb-1">Fantasilandia</h1>
        <p class="text-muted mb-0">Gestión de juegos y atracciones del parque</p>
      </div>

      <!-- Formulario -->
      <div class="col-12 col-lg-5">
        <div class="card shadow-sm">
          <div class="card-body">
            <h2 class="h5 mb-3">Formulario</h2>

            <form @submit.prevent="guardarAtraccion">
              <!-- Nombre -->
              <div class="mb-3">
                <label class="form-label">Nombre del juego</label>
                <input
                  type="text"
                  class="form-control"
                  placeholder="Ej: Raptor Rider"
                  v-model.trim="formulario.nombre"
                />
              </div>

              <!-- Capacidad -->
              <div class="mb-3">
                <label class="form-label">Capacidad de personas</label>
                <input
                  type="number"
                  class="form-control"
                  placeholder="Ej: 24"
                  v-model.number="formulario.capacidad"
                  min="0"
                />
              </div>

              <!-- Ambiente -->
              <div class="mb-3">
                <label class="form-label">Zona del parque</label>
                <select class="form-select" v-model="formulario.ambiente">
                  <option>Zona Familiar</option>
                  <option>Zona Infantil</option>
                  <option>Zona Extrema</option>
                  <option>Zona Acuática</option>
                  <option>Zona Terror</option>
                </select>
              </div>

              <!-- Intensidad -->
              <div class="mb-3">
                <label class="form-label d-block">Nivel de la intensidad</label>

                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="radio"
                    id="int-suave"
                    value="Suave"
                    v-model="formulario.intensidad"
                  />
                  <label class="form-check-label" for="int-suave">Suave</label>
                </div>

                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="radio"
                    id="int-moderado"
                    value="Moderado"
                    v-model="formulario.intensidad"
                  />
                  <label class="form-check-label" for="int-moderado">Moderado</label>
                </div>

                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="radio"
                    id="int-extremo"
                    value="Extremo"
                    v-model="formulario.intensidad"
                  />
                  <label class="form-check-label" for="int-extremo">Extremo</label>
                </div>
              </div>

              <!-- Checkbox -->
              <div class="mb-3 form-check">
                <input
                  class="form-check-input"
                  type="checkbox"
                  id="operativa"
                  v-model="formulario.operativa"
                />
                <label class="form-check-label" for="operativa">
                  ¿Está operativa la atracción?
                </label>
              </div>

              <!-- Descripción -->
              <div class="mb-3">
                <label class="form-label">Descripción del juego</label>
                <textarea
                  class="form-control"
                  rows="3"
                  placeholder="Describe la experiencia del juego..."
                  v-model="formulario.descripcion"
                ></textarea>
              </div>

              <button
                type="submit"
                class="btn btn-primary w-100"
                :disabled="!formulario.nombre"
              >
                Guardar Atracción
              </button>
            </form>
          </div>
        </div>
      </div>

      <!-- Tabla -->
      <div class="col-12 col-lg-7">
        <div class="card shadow-sm">
          <div class="card-body">
            <div class="d-flex align-items-center justify-content-between mb-3">
              <h2 class="h5 mb-0">Listado de Atracciones</h2>
              <span class="badge text-bg-secondary">
                Total: {{ atracciones.length }}
              </span>
            </div>

            <div
              v-if="atracciones.length === 0"
              class="alert alert-warning mb-0"
            >
              No hay atracciones registradas todavía.
            </div>

            <div v-else class="table-responsive">
              <table class="table table-striped align-middle">
                <thead>
                  <tr>
                    <th>#</th>
                    <th>Nombre</th>
                    <th>Capacidad</th>
                    <th>Zona</th>
                    <th>Intensidad</th>
                    <th>Operativa</th>
                    <th></th>
                  </tr>
                </thead>

                <tbody>
                  <tr v-for="(a, index) in atracciones" :key="index">
                    <td>{{ index + 1 }}</td>
                    <td>
                      <strong>{{ a.nombre }}</strong>
                      <div class="text-muted small text-truncate">
                        {{ a.descripcion || "Sin descripción" }}
                      </div>
                    </td>
                    <td>{{ a.capacidad ?? "—" }}</td>
                    <td>{{ a.ambiente }}</td>
                    <td>{{ a.intensidad }}</td>
                    <td>{{ a.operativa ? "Sí" : "No" }}</td>
                    <td>
                      <button
                        class="btn btn-sm btn-outline-danger"
                        @click="eliminarAtraccion(index)"
                      >
                        Eliminar
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.text-truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>
