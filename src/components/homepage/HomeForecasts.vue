<script setup lang="ts">
import { fromZonedTime, toZonedTime } from 'date-fns-tz'

// Get the user's local timezone
const userTimezone = Intl.DateTimeFormat().resolvedOptions().timeZone
console.log("User's Timezone:", userTimezone)

const props = defineProps({
  weatherDataForecast: Object
})

console.log('From forecast props', props.weatherDataForecast)

// Decode weather codes to descriptions
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

// Decode weather code to description
function decodeWeatherCode(code: number): string {
  return weatherCodeDescriptions[code] || 'Unknown weather condition'
}

// Function to convert the forecast time to the user's local timezone
const convertToUserTimezone = (forecastTime: string, userTimezone: string) => {
  const utcTime = fromZonedTime(forecastTime, 'UTC') // Convert from UTC to user's local time
  const localTime = toZonedTime(utcTime, userTimezone)
  return localTime
}

// Format time to 12-hour format for display
const formatTime = (time: Date) => {
  return time.toLocaleString('en-US', {
    hour: 'numeric',
    minute: 'numeric',
    hour12: true // 12-hour format
  })
}

// Get current local time in ISO format
const currentLocalTime = new Date().toISOString()
console.log('Current local time:', currentLocalTime)

// Map weather data and convert forecast times to the user's local time zone
const mappedWeatherDataForecasts =
  props.weatherDataForecast?.precipitation_probability
    .map((_: any, index: number) => {
      const forecastTime = props.weatherDataForecast?.time[index]

      // Convert the forecast time to user's local time zone
      const localTime = convertToUserTimezone(forecastTime, userTimezone)

      // Only include forecasts from the current time onward
      if (forecastTime >= currentLocalTime) {
        return {
          time: formatTime(localTime), // Format the time in 12-hour format
          precipitation_probability: props.weatherDataForecast?.precipitation_probability[index],
          temperature_2m: props.weatherDataForecast?.temperature_2m[index],
          weather_code: decodeWeatherCode(props.weatherDataForecast?.weather_code[index])
        }
      }
    })
    .filter((forecast: undefined) => forecast !== undefined) || []

console.log(mappedWeatherDataForecasts)
</script>

<template>
  <div class="mb-64">
    <h3 class="text-4xl font-semibold pb-2 text-slate-100">Forecasts:</h3>
    <ul class="flex overflow-x-auto">
      <li
        class="ml-3 px-auto py-5 my-3 rounded-lg bg-blue-400 shadow-xl hover:bg-blue-900 duration-200"
        v-for="(forecast, index) in mappedWeatherDataForecasts"
        :key="index"
      >
        <div class="flex flex-col justify-evenly items-center text-slate-100 w-16 h-32 mx-5">
          <div>{{ forecast.time }}</div>
          <div class="text-2xl font-semibold">{{ forecast.temperature_2m }}°C</div>
          <div>{{ forecast.weather_code }}</div>
        </div>
      </li>
    </ul>
  </div>
</template>
