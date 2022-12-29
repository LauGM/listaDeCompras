<script setup>
import { ref, computed, onBeforeMount, onBeforeUnmount, onUpdated } from 'vue';

const encabezado = "Lista de compras";
const nuevo = ref("");
const lista = ref([]);
const prioridad=ref("");
function agregar() {
    lista.value.push(nuevo.value + prioridad.value);
    nuevo.value = "";
    prioridad.value="";
}
function eliminar(index) {
    lista.value.splice(index, 1);
}
const calcularCaracteres = computed(() => nuevo.value.length);
onBeforeMount(() => {
    console.log("antes")
    lista.value = JSON.parse(localStorage.getItem("lista")) || [];
})
onUpdated(() => {
    console.log("despues")
    localStorage.setItem("lista", JSON.stringify(lista.value));
});
</script>

<template>
    <div>
        <h1>{{ encabezado }}</h1>
        <input type="text" placeholder="nuevo producto" v-model="nuevo">
        <label style="margin-left: 5px" for="prioridad">Prioridad:</label>
        <!-- //opcional prioridad -->
        <select v-model="prioridad" name="prioridad">
            <option value=" 🔴">alta</option>
            <option value=" 🟡">media</option>
            <option selected value=" 🟢">baja</option>
        </select>
        <button v-show="nuevo && prioridad" @click="agregar()">Agregar</button>
        <p style="margin-top: 0; font-size:smaller;">{{ calcularCaracteres }} / 50</p>
        <div v-for="(producto, index) of lista" :key="index">
            <p style="display:inline-block">{{ `${index + 1} - ${producto}` }}</p>
            <button style="display:inline-block" @click="eliminar(index)">X</button>
        </div>
    </div>
</template>

<style scoped>

</style>
