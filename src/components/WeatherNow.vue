<template>
    <table style="color: white;">
        <tr>
            <td>
                <div v-for="weather in weatherData">
                    <div style="font-size: 25px;">Прогноз погоды в {{ weather.city }}, {{ weather.country }}</div>
                    <div style="font-size: 20px;">сегодня {{ weather.date }}</div>
                    <img align="left" v-bind:src="weather.pic">
                    <div style=" font-size: 72px;">{{ weather.temp }}°C</div>
                    Ощущается как: {{ weather.feels }}°C
                </div>
            </td>
            <td>
                <div v-for="weather in weatherData" style=" margin-top: 70px; width: 250px;">
                    ветер {{ weather.wind }} м/с<br>
                    давление {{ weather.grnd }} мм.рт.ст.<br>
                    видимость {{ weather.visibility }} км<br>
                    влажность {{ weather.humidity }}%<br>
                    вероятность осадков {{ weather.pop }}%
                </div>
            </td>
            <td>
                <div v-for="weather in weatherData" style=" margin-top: 70px; width: 130px;">
                    {{ weather.time2 }}<br>
                    <img align="left" v-bind:src="weather.pic2">
                    <div style="font-size: 30px;">{{ weather.temp2 }}°C</div>
                </div>
            </td>
            <td>
                <div v-for="weather in weatherData" style=" margin-top: 70px; width: 200px;">
                    💨 {{ weather.wind2 }} м/с<br>
                    🗜️ {{ weather.grnd2 }} мм.рт.ст.<br>
                    🔭 {{ weather.visibility2 }} км<br>
                    💧 {{ weather.humidity2 }}%<br>
                    ☂️ {{ weather.pop2 }}%
                </div>
            </td>
            <td>
                <div v-for="weather in weatherData" style=" margin-top: 70px; width: 150px;">
                    {{ weather.time3 }}<br>
                    <img align="left" v-bind:src="weather.pic2">
                    <div style="font-size: 30px;">{{ weather.temp3 }}°C</div>
                </div>
            </td>
            <td>
                <div v-for="weather in weatherData" style=" margin-top: 70px;">
                    💨 {{ weather.wind3 }} м/с<br>
                    🗜️ {{ weather.grnd3 }} мм.рт.ст.<br>
                    🔭 {{ weather.visibility3 }} км<br>
                    💧 {{ weather.humidity3 }}%<br>
                    ☂️ {{ weather.pop3 }}%
                </div>
            </td>
        </tr>

    </table>
</template>
  
<script setup>
import axios from 'axios'
import { ref, inject, watch, onMounted } from 'vue'

const city = inject("city")
defineProps({
    type: Object,
    required: true
})
const weatherData = ref([])
watch(city, () => {
    getWeather()
})
function getWeather() {
    const cityValue = city.value
    let Data = new Date();
    let Year = Data.getFullYear();
    let Month = Data.getMonth();
    let indexMonth = ['января', 'февраля', 'марта', 'апреля', 'мая', 'июня', 'июля', 'августа', 'сентября', 'октября', 'ноября', 'декабря'];
    let Ned = Data.getDay();
    let indexNed = ['воскресенье', 'понедельник', 'вторник', 'среда', 'четверг', 'пятница', 'суббота']
    let Day = Data.getDate();
    const date = (indexNed[Ned] + ", " + Day + " " + indexMonth[Month] + " " + Year)
    let indexTime = ['ночью', 'вечером', 'днем', 'ночью', '4', 'днем', 'утром', '7', 'вечером', 'утром']
    axios.get(`https://api.openweathermap.org/data/2.5/forecast?${cityValue}&units=metric&appid=dd942f90e8c353bb0a469a7db5bbb3d4`).then((res) => {
        weatherData.value = [{
            city: res.data.city.name,
            country: res.data.city.country,
            temp: Math.round(res.data.list[0].main.temp),
            pic: import.meta.env.BASE_URL + "/big/" + res.data.list[0].weather[0].main + ".png",
            date: date,
            feels: Math.round(res.data.list[0].main.feels_like),
            wind: Math.round(res.data.list[0].wind.speed),
            grnd: Math.round(res.data.list[0].main.grnd_level / 1.333),
            visibility: (res.data.list[0].visibility) / 1000,
            humidity: res.data.list[0].main.humidity,
            pop: Math.round(res.data.list[0].pop * 100),

            time2: indexTime[res.data.list[2].dt_txt.slice(12, 13)],
            pic2: import.meta.env.BASE_URL + "/min/" + res.data.list[2].weather[0].main + ".png",
            temp2: Math.round(res.data.list[2].main.temp),
            wind2: Math.round(res.data.list[2].wind.speed),
            grnd2: Math.round(res.data.list[2].main.grnd_level / 1.333),
            visibility2: (res.data.list[2].visibility) / 1000,
            humidity2: res.data.list[2].main.humidity,
            pop2: Math.round(res.data.list[2].pop * 100),

            time3: indexTime[res.data.list[4].dt_txt.slice(12, 13)],
            pic3: import.meta.env.BASE_URL + "/min/" + res.data.list[4].weather[0].main + ".png",
            temp3: Math.round(res.data.list[4].main.temp),
            wind3: Math.round(res.data.list[4].wind.speed),
            grnd3: Math.round(res.data.list[4].main.grnd_level / 1.333),
            visibility3: (res.data.list[4].visibility) / 1000,
            humidity3: res.data.list[4].main.humidity,
            pop3: Math.round(res.data.list[4].pop * 100),
        }]
    })
}
// onMounted(() => getWeather())
</script>