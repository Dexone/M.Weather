<template>
  <div id="block" v-for="weather in weatherInfo" style="width: 120px; display: inline-block; font-size: 18px; text-align: center; color: white; ">
    <table cellspacing="20">
      <td>{{ weather.day }}</td>
      <tr>
        <td><img v-bind:src="weather.pic"></td>
      </tr>
      <tr>
        <td>{{ weather.temp }}°C</td>
      </tr>
      <tr>
        <td>💨 {{ weather.wind }}м/с</td>
      </tr>
      <tr>
        <td>🗜️ {{ weather.grnd }} мм</td>
      </tr>
      <tr>
        <td>🔭 {{ weather.visibility }} км</td>
      </tr>
      <tr>
        <td>💧 {{ weather.humidity }}%</td>
      </tr>
      <tr>
        <td>☂️ {{ weather.pop }}%</td>
      </tr>
    </table>

  </div>
</template>

<script setup>
import axios from 'axios'
import { ref, inject, watch, onMounted } from 'vue'

const city = inject("city")
defineProps({
  type: Object,
  required: true
})
const weatherInfo = ref([])
// console.log(weatherInfo)
watch(city, () => {
  getWeather()
})
function getWeather() {
  const cityValue = city.value
  axios.get(`https://api.openweathermap.org/data/2.5/forecast?${cityValue}&units=metric&appid=dd942f90e8c353bb0a469a7db5bbb3d4`).then((res) => {
    const weatherData = res.data.list.map((item, index) => {
      return {
        day: res.data.list[index].dt_txt.slice(8, -9) + "." + res.data.list[index].dt_txt.slice(5, -12),
        pic: "/src/assets/min/" + res.data.list[index].weather[0].main + ".png",
        temp: Math.round(res.data.list[index].main.temp),
        wind: Math.round(res.data.list[index].wind.speed),
        grnd: Math.round(res.data.list[index].main.grnd_level/1.333),
        visibility: (res.data.list[index].visibility)/1000,
        humidity: res.data.list[index].main.humidity,
        pop: Math.round(res.data.list[index].pop*100),
      }
    })
    weatherInfo.value = weatherData; weatherData.splice(0, 8); weatherData.splice(1, 7); weatherData.splice(2, 7); weatherData.splice(3, 7); weatherData.splice(4, 6)

  })
}
// onMounted(() => getWeather())

</script>