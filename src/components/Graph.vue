<template>
    <LineChart :chartData="lineData" style="width:335px; height: 290px;background-color: rgba(0, 0, 0, 0.7)" />
</template>

<script setup>
import axios from 'axios'
import Chart from 'chart.js/auto';
import { LineChart } from "vue-chart-3"
import { ref, computed, inject, watch, onMounted } from 'vue'
const city = inject("city")
defineProps({
    type: Object,
    required: true
})
watch(city, () => {
    drawGraph()
})
function drawGraph() {
    const cityValue = city.value
    axios.get(`https://api.openweathermap.org/data/2.5/forecast?${cityValue}&units=metric&appid=dd942f90e8c353bb0a469a7db5bbb3d4`).then((res) => {
        x.value = ["Now", res.data.list[1].dt_txt.slice(11, -6), res.data.list[2].dt_txt.slice(11, -6), res.data.list[3].dt_txt.slice(11, -6), res.data.list[4].dt_txt.slice(11, -6), res.data.list[5].dt_txt.slice(11, -6), res.data.list[6].dt_txt.slice(11, -6), res.data.list[7].dt_txt.slice(11, -6), res.data.list[8].dt_txt.slice(11, -6)]
        y.value = [res.data.list[0].main.temp, res.data.list[1].main.temp, res.data.list[2].main.temp, res.data.list[3].main.temp, res.data.list[4].main.temp, res.data.list[5].main.temp, res.data.list[6].main.temp, res.data.list[7].main.temp, res.data.list[8].main.temp]
    })
}
const x = ref([])
const y = ref([])
const lineData = computed(() => ({
    labels: x.value,
    boxWidth: 0,
    datasets: [
        {

            data: y.value,
            label: 'Температура на 24 часа',
            borderColor: 'rgb(255, 255, 255)', //цвет линии
            borderWidth: 1, // толщина линии
            backgroundColor: 'rgba(255, 255, 255, 0.0)', //точки
        },
    ],
}));
// onMounted(() => drawGraph())
</script>