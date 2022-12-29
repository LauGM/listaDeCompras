<script setup>
    import { ref, computed } from 'vue';

    const encabezado = "Lista de compras";
    const nuevo=ref("");
    const lista=ref([]);
    function agregar(){
        lista.value.push(nuevo.value);
        nuevo.value=""
    }
    function eliminar(index){
        lista.value.splice(index,1);
    }
    const calcularCaracteres=computed(()=>nuevo.value.length);
</script>

<template>
    <div>
        <h1>{{ encabezado }}</h1>
        <input type="text" placeholder="nuevo producto" v-model="nuevo">
        <button @click="agregar()">Agregar</button>
        <p style="margin-top: 0; font-size:smaller;">{{ calcularCaracteres }} / 50</p>
        <div v-for="(producto,index) of lista" :key="index">
            <p style="display:inline-block">{{ `${index} - ${producto }` }}</p>
            <button style="display:inline-block" @click="eliminar(index)">X</button>
        </div>
        
    </div>
</template>

<style scoped>

</style>
