<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'
import HomeHeader from '../components/homepage/HomeHeader.vue'
import HomeCurrentWeather from '../components/homepage/HomeCurrentWeather.vue'
import HomeForecasts from '../components/homepage/HomeForecasts.vue'
import HomeFooter from '../components/homepage/HomeFooter.vue'

const weatherDataCurrent = ref()
const coords = ref()
const weatherDataForecast = ref()

if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition((position) => {
    const latitude = position.coords.latitude
    const longitude = position.coords.longitude
    if (latitude && longitude) {
      coords.value = {
        latitude,
        longitude
      }
      axios
        .get(
          `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current_weather&hourly=temperature_2m,precipitation_probability,weather_code&forecast_days=1&timezone=auto`
        )
        .then((response) => {
          weatherDataCurrent.value = response.data.current_weather
          weatherDataForecast.value = response.data.hourly
        })
        .catch((error) => {
          console.error(error)
        })
    }
  })
} else {
  console.log('geolocation is not allowed')
}

console.log('weather data: ', weatherDataCurrent)
</script>

<template>
  <main class="container mx-auto">
    <h1 v-if="coords">
      <HomeHeader :coords="coords" />
    </h1>
    <h1 v-if="weatherDataCurrent">
      <HomeCurrentWeather :weatherDataCurrent="weatherDataCurrent" />
    </h1>
    <h1 v-if="weatherDataForecast">
      <HomeForecasts :weatherDataForecast="weatherDataForecast" />
    </h1>
    <footer>
      <HomeFooter />
    </footer>
  </main>
</template>
