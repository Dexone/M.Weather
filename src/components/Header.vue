<template>
    <div style="background-color: rgba(0, 0, 0, 0.9);height: 80px;padding-left: 300px;padding-top: 20px;font-size: 26px;">
        M.WEATHER
        <button @click=latlong class="button is-success is-rounded"><img src="/geo.png"></button>
        <input class="input" id="inp" type="text" placeholder="Введите название города"
            style="width: 300px; margin-right: 5px; margin-left: 5px;">
        <button id="butt" class="button is-danger is-rounded" style="width: 5px; margin-right: 5px;">❤️</button>
        <button @click=name class="button">Найти</button>

    </div>
</template>

<script setup>
import { ref, inject, watch, onMounted } from 'vue'

defineProps({
    type: Object,
    required: true
})
const city = inject("city")

function name() {
    let inputCity = document.querySelector("#inp").value
    city.value = "q=" + inputCity
}

function latlong() {
    navigator.geolocation.getCurrentPosition(function (position) {
        let lat = position.coords.latitude;
        let lon = position.coords.longitude;
        city.value = "lat=" + lat + "&lon=" + lon
    });
}
</script>