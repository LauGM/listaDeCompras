<script setup>
import { ref, computed, onUpdated, onBeforeMount, onBeforeUpdate } from 'vue';

const encabezado = "Lista de compras";
const nuevo = ref("");
const lista = ref([]);
const filtro = ref([]);
const prioridad = ref("");
const categoria = ref("");
const radioCat = ref("");
function agregar() {
    lista.value.push(nuevo.value + prioridad.value + categoria.value);
    nuevo.value = "";
    prioridad.value = "";
    categoria.value = "";
    localStorage.setItem("lista", JSON.stringify(lista.value));
}
function eliminar(index) {
    console.log("+++++ Eliminando ++++++")
    let nombre = filtro.value[index];
    filtro.value.splice(index, 1);
    lista.value.splice(lista.value.indexOf(nombre), 1);
    console.log("elimino " + nombre + " del filtro y de la lista")
    localStorage.setItem("lista", JSON.stringify(lista.value));
}
const calcularCaracteres = computed(() => nuevo.value.length);
onBeforeMount(() => {
    lista.value = JSON.parse(localStorage.getItem("lista")) || [];
    filtro.value = lista.value;
})
onBeforeUpdate(() => {
    filtro.value = lista.value.filter((prod) => prod.includes(radioCat.value));
})
</script>

<template>
    <div>
        <h1>{{ encabezado }}</h1>
        <label for="nuevo">Producto: </label>
        <input type="text" placeholder="nuevo producto" name="nuevo" v-model="nuevo">
        <p style="margin-top: 0; font-size:smaller;">{{ calcularCaracteres }} / 50</p>
        <div style="display:inline-block">
            <!-- //opcional prioridad -->
            <label style="margin-left: 5px" for="prioridad">Prioridad: </label>
            <select v-model="prioridad" name="prioridad">
                <option value="" disabled>Selecciona</option>
                <option value=" 🔴">alta</option>
                <option value=" 🟡">media</option>
                <option value=" 🟢">baja</option>
            </select>
        </div>
        <div style="display:inline-block">
            <!-- //opcional categoría -->
            <label style="margin-left: 5px" for="categoria">Categoría: </label>
            <select v-model="categoria" name="categoria">
                <option value="" disabled>Selecciona</option>
                <option value=" 🛒">almacén</option>
                <option value=" 🥕">verdulería</option>
                <option value=" 🍖">carnicería</option>
                <option value=" 🥐">panadería</option>
                <option value=" 🛍️">otro</option>
            </select>
        </div>

        <button v-show="nuevo && prioridad && categoria" @click="agregar()">Agregar</button>
        <hr />
        <div>
            <label>Filtrar por:</label>
            <label>Nada
                <input v-model="radioCat" type="radio" id="ninguno" name="ninguno" value="" selected>
            </label>
            <br>
            <label> 🛒
                <input v-model="radioCat" type="radio" id="almacen" name="almacen" value="🛒">
            </label>
            <label>| 🥕
                <input v-model="radioCat" type="radio" id="verduleria" name="verduleria" value="🥕">
            </label>
            <label>| 🍖
                <input v-model="radioCat" type="radio" id="carniceria" name="carniceria" value="🍖">
            </label>
            <label>| 🥐
                <input v-model="radioCat" type="radio" id="panaderia" name="panaderia" value="🥖">
            </label>
            <label>| 🛍️
                <input v-model="radioCat" type="radio" id="otro" name="otro" value="🛍️">
            </label>
        </div>
        <hr />
        <div v-for="(producto, index) of filtro" :key="index">
            <p style="display:inline-block">{{ `${index + 1} - ${producto}` }}</p>
            <button style="display:inline-block" @click="eliminar(index)">X</button>
        </div>
    </div>
</template>

<style scoped>
    label{
        margin-right:2px;
    }
</style>
