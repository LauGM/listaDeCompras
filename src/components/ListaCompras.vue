<script setup>
    import { ref, computed, onBeforeMount, onBeforeUnmount, onUpdated } from 'vue';

    const encabezado = "Lista de compras";
    const nuevo=ref("");
    const lista=ref([]);
    function agregar(){
        lista.value.push(nuevo.value);
        nuevo.value="";
    }
    function eliminar(index){
        lista.value.splice(index,1);
    }
    const calcularCaracteres=computed(()=>nuevo.value.length);
    onBeforeMount(()=>{
        console.log("antes")
        lista.value=JSON.parse(localStorage.getItem("lista"))||[];
    })
    onUpdated(()=>{
        console.log("despues")
        localStorage.setItem("lista",JSON.stringify(lista.value));
    });
</script>

<template>
    <div>
        <h1>{{ encabezado }}</h1>
        <input type="text" placeholder="nuevo producto" v-model="nuevo">
        <button @click="agregar()">Agregar</button>
        <p style="margin-top: 0; font-size:smaller;">{{ calcularCaracteres }} / 50</p>
        <div v-for="(producto,index) of lista" :key="index">
            <p style="display:inline-block">{{ `${index+1} - ${producto }` }}</p>
            <button style="display:inline-block" @click="eliminar(index)">X</button>
        </div>
    </div>
</template>

<style scoped>

</style>
