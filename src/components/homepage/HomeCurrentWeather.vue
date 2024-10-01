<script setup lang="ts">
const props = defineProps({
  weatherDataCurrent: Object // Expecting weatherData to be an object
})

const weatherCodeDescriptions: Record<number, string> = {
  0: 'Clear sky',
  1: 'Mainly clear',
  2: 'Partly cloudy',
  3: 'Overcast',
  45: 'Fog',
  48: 'Depositing rime fog',
  51: 'Drizzle: Light intensity',
  53: 'Drizzle: Moderate intensity',
  55: 'Drizzle: Dense intensity',
  56: 'Freezing Drizzle: Light intensity',
  57: 'Freezing Drizzle: Dense intensity',
  61: 'Rain: Slight intensity',
  63: 'Rain: Moderate intensity',
  65: 'Rain: Heavy intensity',
  66: 'Freezing Rain: Light intensity',
  67: 'Freezing Rain: Heavy intensity',
  71: 'Snowfall: Slight intensity',
  73: 'Snowfall: Moderate intensity',
  75: 'Snowfall: Heavy intensity',
  77: 'Snow grains',
  80: 'Rain showers: Slight intensity',
  81: 'Rain showers: Moderate intensity',
  82: 'Rain showers: Violent intensity',
  85: 'Snow showers: Slight intensity',
  86: 'Snow showers: Heavy intensity',
  95: 'Thunderstorm: Slight or moderate',
  96: 'Thunderstorm with slight hail',
  99: 'Thunderstorm with heavy hail'
}

function decodeWeatherCode(code: number): string {
  return weatherCodeDescriptions[code] || 'Unknown weather condition'
}

const temperature: number = props.weatherDataCurrent?.temperature
const weatherCode: number = props.weatherDataCurrent?.weathercode
const weatherCondition: string = decodeWeatherCode(weatherCode)
const windDirection: number = props.weatherDataCurrent?.winddirection
const windSpeed: number = props.weatherDataCurrent?.windspeed
</script>

<template>
  <div>
    <div class="my-20 text-xl text-slate-200 font-normal" v-if="props.weatherDataCurrent">
      <h1 class="text-8xl pt-4 pb-2 font-extrabold text-slate-100">{{ temperature }}°C</h1>
      <h3 class="text-l pb-1">{{ weatherCondition }}</h3>
      <h3 class="text-l pb-1">Wind Direction: {{ windDirection }}°</h3>
      <h3 class="text-l pb-1">Wind Speed: {{ windSpeed }}km/h</h3>
    </div>
  </div>
</template>
