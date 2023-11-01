<template>
    Погода в моих городах:
    <table cellspacing="20">
        <tr>
            <td style="width: 210px;">
                <div v-for="favourite, index in listFavourites">
                    <button style="width: 5px; height: 20px;" @click="listFavourites.splice(index, 1)"
                        class="button">❌</button>
                    <button style="width: 5px; height: 20px;" @click="chooseCity = listFavourites[index]"
                        v-on:click="searchFavourite" class="button">🔍</button> {{ favourite }}
                </div>
            </td>
            <td style="width: 40px;">
                <div v-for="temp in temps">{{ temp }}°C</div>

            </td>
            <td>
                <div v-for="pic in pics"> <img v-bind:src="pic"></div>
            </td>
        </tr>
    </table>
</template>
<script setup>
import axios from 'axios'
import { ref, inject, watch, onMounted } from 'vue'
import { useStorage } from '@vueuse/core'
import { useThrottleFn } from '@vueuse/core'

const city = inject("city")
defineProps({
    type: Object,
    required: true
})
const listFavourites = ref([])
const state = useStorage('vue-use-local-storage', listFavourites)
const button = document.querySelector("#butt") //   ну что это такое то кринж
button.onclick = function addFavourite() {
    let input = document.querySelector("#inp").value
    listFavourites.value.push(input);
}
const chooseCity = ref()
function searchFavourite() {
    city.value = "q=" + chooseCity.value
}
const temps = ref([])
const pics = ref([])
watch(listFavourites.value, () => {
    update()
})
function update() {
    temps.value.length = 0
    pics.value.length = 0
    let count = listFavourites.value.length
    let i = 0
    const throttledFn = useThrottleFn(() => {
        while (i < count) {
            axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=${listFavourites.value[i]}&units=metric&appid=dd942f90e8c353bb0a469a7db5bbb3d4`).then((res) => {
                temps.value.push(Math.round(res.data.list[0].main.temp))
                pics.value.push(import.meta.env.BASE_URL+'/minimin/' + res.data.list[0].weather[0].main + ".png")
            })
            i++
        }
    }, 1000)

    window.addEventListener('resize', throttledFn)
    console.log(pics)
}
onMounted(() => update())
</script>