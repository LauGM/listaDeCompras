<template>
    <p style="font-size: 12px;">Clima en {{ clima.place.name }}</p>
    <p style="font-size: 12px;">Min: {{ clima.periods[0].minTempC }} grados | Max: {{ clima.periods[0].maxTempC }} grados | {{clima.periods[0].weather }}</p>
</template>

<script setup>
import { ref } from 'vue';
const clima = ref(null)
const options = {
    method: 'GET',
    headers: {
        'X-RapidAPI-Key': 'df5eac0bd2msh339a8446e79ab6ep12e415jsn930a726e354a',
        'X-RapidAPI-Host': 'aerisweather1.p.rapidapi.com'
    }
};

clima.value = await fetch('https://aerisweather1.p.rapidapi.com/forecasts/buenosaires,ar', options).then((response) => response.json())
    .then(response => response.response[0])
    .catch(err => console.error(err));
</script>

<style scoped>

</style>