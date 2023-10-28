<template>
  <div>
    <Header />
  </div>
  <div
    style="width: 1300px; background-color: rgba(0, 0, 0, 0.7); margin-left: auto; margin-right: auto; margin-top: 10px; height: 250px; padding: 20px;  ">
    <WeatherNow />
  </div>

  <table cellspacing="20" style="margin-left: 295px; margin-top: 10px;">
    <tr>
      <td>
        <div
          style="  width: 640px; height: 290px; background-color: rgba(0, 0, 0, 0.7); padding: 20px; margin-right: 10px;">
          <Weather5day />
        </div>
      </td>
      <td>
        <div style="margin-right: 10px;">
          <Graph />
        </div>
      </td>
      <td>
        <div
          style="width: 305px; height: 290px; background-color: rgba(0, 0, 0, 0.7); padding: 20px; display: inline-block; color: white; ">
          <FavouriteCity />
        </div>
      </td>
    </tr>
  </table>
</template>

<script setup>
import Weather5day from "./components/Weather5day.vue"
import Graph from "./components/Graph.vue"
import WeatherNow from "./components/WeatherNow.vue"
import FavouriteCity from "./components/FavouriteCity.vue"
import Header from "./components/Header.vue"

import { provide, ref } from 'vue'
const city = ref()
provide("city", city)



</script>




