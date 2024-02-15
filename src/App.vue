

<template>
  <div class="container">
    <div class="registrioPrincipal">
      <div class="registroLeft">
        <div class="titulo">
          <h1> Ordenador Semanal</h1>
        </div>
        <div class="alert" v-if="alerta != ''">
          <h2
            :style="alerta === 'Registro Exitoso' ? 'background-color:green;color:white' : 'background-color:red; color:white'">
            {{ alerta }}</h2>

        </div>
        <div class="registro">

          <label class="tareas">
            <p>Digite su Tarea a Registrar</p>
            <input type="text" v-model="nombre">
          </label>
          <label class="tareas">
            <p>Seleccione fecha de registro</p>
            <input type="date" v-model="fecha" required>
          </label>
          <label>
            <p>Prioridad </p>
            <label>
              <input type="radio" v-model="prioridad" name="prioridad" value="alta">Alta
            </label>
            <label>
              <input type="radio" v-model="prioridad" name="prioridad" value="baja">Baja
            </label>

          </label><br>
        </div>
        <div class="registroRith">
          <label>
            <input class="buttonAgregar" type="button" value="Agregar" @click="validar()">
          </label>

          <label>
            <input class="ordenar" type="button" value="Ordenar" @click="ordenar()"></label>

        </div>
      </div>
    </div>

    <div class="tabla">
      <label>
        <p>Tabla de Registro</p>
      </label>
      <table>
        <thead>
          <tr class="encabezadoTabla">
            <th>Actividad</th>
            <th>Prioridad</th>
            <th>Fecha</th>
            <th>Opciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, i) in citas" :key="i"
            :style="item.prioridad == 'alta' ? ' background-color:red; color:white' : ' '">

            <td>{{ item.nombre }}</td>
            <td>{{ item.prioridad }}</td>
            <td>{{ item.fecha }}</td>
            <td>
              <button @click="eliminar(i)">❌</button>
              <button @click="editar(item, i)">📝</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>



  </div>
</template>


<script setup>

import { ref } from 'vue';

const citas = ref([])

let nombre = ref("")
let fecha = ref("")
let prioridad = ref("")
let alerta = ref("")


function registrar() {
  const registro = {
    nombre: nombre.value,
    fecha: fecha.value,
    prioridad: prioridad.value
  }
  citas.value.push(registro)
  console.log(citas.value)
  limpiar()
}

function limpiar() {
  nombre.value = "";
  fecha.value = "";
  prioridad.value = "";
}

function ordenar() {
  citas.value.sort((a, b) => {
    if (a.prioridad === b.prioridad) {
      return new Date(a.fecha) - new Date(b.fecha);
    }
    return a.prioridad === 'alta' ? -1 : 1;
  });
}
function eliminar(i) {
  citas.value.splice(i, 1)
}
function editar(item, i) {
  console.log(item);
  console.log(i);
  nombre.value = item.nombre
  fecha.value = item.fecha
  prioridad.value = item.prioridad


}
function validar() {
  const fechaActual = new Date();
  const fechaSeleccionanda = new Date(fecha.value);
  if (nombre.value === "") {
    alerta.value = 'Nombre no debe estar vacio'
    setTimeout(() => {
      alerta.value = ""
    }, 3000);
  }
  else if (fecha.value == "") {
    alerta.value = 'seleccione fecha'
    setTimeout(() => {
      alerta.value = ""
    }, 3000);
  }
  else if (prioridad.value == "") {
    alerta.value = 'seleccione prioridad'
    setTimeout(() => {
      alerta.value = ""
    }, 3000);
  }
  else if (fechaActual >= fechaSeleccionanda) {
    alerta.value = 'fecha incorrecta'
    setTimeout(() => {
      alerta.value = ""
    }, 3000);
  }
  else {
    registrar()
    alerta.value = 'Registro Exitoso'
    setTimeout(() => {
      alerta.value = ""
    }, 3000);
  }
}
</script>

<style >
* {
  margin: 0;
  padding: 0;
  font-family: Arial, Helvetica, sans-serif;
}


.titulo {

  width: 100%;
  text-align: center;
  padding: 2%;
  background-color: #c9caca;
}

.registro {
  background-color: #ecf3f3;
  padding-left: 30%;
}

p {
  font-size: 120%;
  margin: 1%;
}

input[type="text"] {
  width: 40%;
  height: 30px;
  border: none;
  border-radius: 5px;
  background-color: #d9dbdb;
  font-size: 130%;
  padding-left: 1%;
}

input[type="date"] {
  width: 20%;
  height: 30px;
  border-radius: 5px;
  margin-left: 1%;
  padding-left: 2%;
  padding-right: 2%;
  font-size: 130%;
}

.buttonAgregar {
  background-color: #2ad82a;
  width: 15%;
  height: auto;
  border: none;
  border-radius: 5px;
  font-size: 120%;
  padding: 1%;
}

.ordenar {
  margin: 1%;
  width: 15%;
  height: auto;
  font-size: 120%;
  border-radius: 5px;
  border: none;
  background-color: #47def1;
  padding: 1%;
}

.tabla {
  text-align: center;
  width: 100%;

}

table {
  margin-left: 10%;
  width: 80%;
  border-collapse: collapse;

}

th,
td,
tr {
  border: 2px solid #6e6262;
  width: 25%;
  padding: 1%;
}

.encabezadoTabla {
  background-color: #47def1;
}

.alert {

  width: 40%;
  height: auto;
  border-radius: 10px;
  margin-left: 30%;
  position: fixed;
}

h2 {
  width: 100%;
  height: 50px;
  padding: 2%;
  text-align: center;
  border-radius: 10px;
}
</style>

  