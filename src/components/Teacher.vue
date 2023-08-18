<script setup>

import { reactive, ref } from 'vue';

const nombre = ref('');
const apellido = ref('');
const dni = ref(null);
const materia = ref('');
const materias = ref([]);
const entregado = ref(false);

/* 
    ERROR QUE TUVE TAMBIÉN YO 
    Meter directamente, el profesor a profesores, entonces la referencia sigue existiendo.
    como solucionar?? crear un objeto no reactivo, a ese elemento no reactivo asignarle los elementos reactivos de ese momento,
    y éste no reactivo agregar a profesores en este caso. Es decir, a profesores.value.push({ name: nombre.value, ... })
*/
/* let profesor = ref(
    {
        nombre: '',
        apellido: '',
        dni: 0,
        materias: [],
        entregado: false
    });
 */

let profesores = ref([]);

/* 
    agregar materia
*/
const agregarMateria = () => {
    
    if (materia.value.length) {
        materias.value.push(materia.value);
        materia.value = '';
    } else {
        alert("Debe ingresar una materia antes de poder agregar");
    }
}

/* 
    agregar profesor
*/
const agregarProfesor = () => { 

    if (entregado.value) {
        const miProfe = {
            nombre: nombre.value,
            apellido: apellido.value,
            dni: dni.value,
            materias: materias.value,
            entregado: entregado.value
        }

        profesores.value.push(miProfe);

        nombre.value = '';
        apellido.value = '';
        dni.value = null,
        materias.value = [];
        entregado.value = false;
    } else {
        alert("Debe entregar la documentación antes ")
    }

}

/* 
    Editar profesor
*/
function editarProfesor(index) {
    const editar = profesores.value[index];

    nombre.value = editar.nombre;
    apellido.value = editar.apellido;
    dni.value = editar.dni;
    materias.value = editar.materias;
    entregado.value = editar.entregado;

    profesores.value = profesores.value.filter((item) => item != editar);
}

/* 
    Eliminar profesor
*/
function eliminarProfesor(index) {
    profesores.value = profesores.value.filter((item) => item != profesores.value[index]);
}

</script>

<template>

    <div class="teacher">

        <h3>Añadir profesor: </h3>
        <form class="form">

            <div class="form__item nombre">
                <label for="nombre">nombre</label>
                <input type="text" name="nombre" id="nombre" required v-model="nombre" placeholder="Ingrese nombre">
            </div>

            <div class="form__item apellido">
                <label for="apellido">apellido</label>
                <input type="text" name="apellido" id="apellido" required v-model="apellido" placeholder="Ingrese apellido">
            </div>

            <div class="form__item dni">
                <label for="dni">DNI</label>
                <input type="number" name="dni" id="dni" required v-model="dni"  placeholder="Ingrese número de documento">
            </div>

            <div class="form__item materias">
                <label for="materias">materias</label>
                <input type="text" name="materias" id="materias" required v-model="materia" placeholder="Ingrese materia que dicta"> 
                <button class="btn-agregar-materia" @click.prevent="agregarMateria">Agregar materia</button>
            </div>

            <div class="materias-agregadas">
                <ul v-for="m, index in materias" :key="index">
                    <li>{{ m }}</li>
                </ul>
            </div>

            <div class="form__item check-doc-entregado">
                <label for="checkDoc">Entrego la documentación?</label>
                <input type="checkbox" name="checkDoc" id="checkDoc" required v-model="entregado">
            </div>

            <button @click.prevent="agregarProfesor" class="btn-agregar">Agregar</button>

        </form>

        <article class="listado">
            <h3>Lista de profesores: </h3>

            <!-- <ul v-if="profesores.length" v-for="profe, index in profesores" :key="index">
                <li>
                    <h3>{{ profe.nombre }} {{ profe.apellido }}</h3>
                    <h3>dni: {{ profe.dni }}</h3>
                    <h3>Si entrego los papeles y dicta las siguientes materias:
                        {{ profe.materias }}
                    </h3>
                </li>
            </ul> -->
            <table v-if="profesores.length" >
                <tr>
                    <th>Nombre y apellido</th>
                    <th>DNI</th>
                    <th>Materias</th>
                    <th>Entrego documentación</th>
                </tr>
                <tr v-for="profe, index in profesores" :key="index">
                    <td>{{ profe.nombre }} {{ profe.apellido }}</td>
                    <td>{{ profe.dni }}</td>
                    <td>{{ profe.materias.join(', ') }}</td>
                    <td>{{ profe.entregado }}</td>

                    <td>
                        <button class="editar-btn btn" @click.prevent="editarProfesor(index)">editar</button>
                        <button class="borrar-btn btn" @click.prevent="eliminarProfesor(index)">borrar</button>
                    </td>
                </tr>
            </table>
            <p v-else>Todavía no hay profesores añadidos</p>
        </article>

    </div>

</template>

<style scoped>

.teacher {
}

.form {
    width: 500px;
    margin: 0 auto;

    display: flex;
    flex-direction: column;

    gap: 10px;

    border: 1px solid white;
    padding: 5px;
    border-radius: 10px;
}

.form__item {
    display: flex;
    justify-content: space-between;
}

.form__item label {
    display: block;
    width: 100px;
}

.form__item input[type="text"], input[type="number"] {
    max-width: 100%;
    padding: 5px;
}

.form__item input[type="checkbox"] {
    width: 50px;
}

.listado {
}

.form__item {
    padding: .5px;
    border: 1px solid transparent;
}

.form__item:hover {
    border-bottom: 1px solid blueviolet;
    border-top: 1px solid blueviolet;
}

.nombre {}

.apellido {}

.dni {}

.materias {}

.materias-agregadas ul {
}

.btn-agregar-materia {
    cursor: pointer;
}

.check-doc-entregado {
}

.btn-agregar {
    border-radius: 10px;
    cursor: pointer;
}

button {
    border: none;
    outline: none;
}

table {
    width: max(90%, 500px);
    text-align: center;

    border: 1px blueviolet solid;

    margin: 0 10px;
    padding: 10px;
}

th {
    border-right: 1px solid blueviolet;
    border-bottom: 1px solid blueviolet;
}
th:last-child {
    border-right: none;
}

td {
    border-right: 1px solid blueviolet;
    border-bottom: 1px solid blueviolet;
}
td:last-child {
    border-right: none;
}

.btn {
    cursor: pointer;
    margin-right: 10px;
}

.editar-btn {}

.borrar-btn {}

</style>