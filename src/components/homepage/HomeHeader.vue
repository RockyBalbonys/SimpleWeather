<script setup lang="ts">
import axios from 'axios'
import { ref } from 'vue'

const props = defineProps({
  coords: Object
})

const loc = ref()

const longitude = props.coords?.longitude
const latitude = props.coords?.latitude

const rgapi = `https://nominatim.openstreetmap.org/reverse?lat=${latitude}&lon=${longitude}&format=json`

axios
  .get(rgapi)
  .then((response) => {
    console.log(response.data)
    loc.value = {
      city: response.data?.address?.city,
      country: response.data?.address?.country
    }
  })
  .catch((err) => {
    console.log(err)
  })
</script>

<template>
  <div>
    <header>
      <h1 class="flex text-4xl py-2 text-slate-100 font-bold">
        <span
          ><img
            src="..\..\assets\WeatherIcons\simpleWeatherIcon.png"
            alt="icon"
            class="h-10 w-10" /></span
        >Simple Weather
      </h1>
      <hr />
      <div v-if="loc">
        <h2 class="text-xl text-slate-200 font-normal ml-10">{{ loc.city }}, {{ loc.country }}</h2>
      </div>
    </header>
  </div>
</template>
