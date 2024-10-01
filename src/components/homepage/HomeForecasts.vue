<script setup lang="ts">
const props = defineProps({
  weatherDataForecast: Object
})

console.log('From forecast props', props.weatherDataForecast)


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

// const temperature: number = props.weatherDataForecast?.temperature
// const weatherCode: number = props.weatherDataForecast?.weather_code
// const weatherCondition: string = decodeWeatherCode(weatherCode)

const formatTime = (timeString: string) => {
  const date = new Date(timeString);
  return date.toLocaleString('en-US', {
    hour: 'numeric',
    minute: 'numeric',
    hour12: false
  });
};

const mappedWeatherDataForecasts =
  props.weatherDataForecast?.precipitation_probability.map((_: any, index: number) => {
    return {
      time: formatTime(props.weatherDataForecast?.time[index]), // Format the time to 12-hour
      precipitation_probability: props.weatherDataForecast?.precipitation_probability[index],
      temperature_2m: props.weatherDataForecast?.temperature_2m[index],
      // weather_code: props.weatherDataForecast?.weather_code[index]
      weather_code: decodeWeatherCode(props.weatherDataForecast?.weather_code[index])

    }
  }) || [] // Default to an empty array if undefined

console.log(mappedWeatherDataForecasts)
</script>


<template>
  <div class="mb-20">
    <ul class="flex overflow-x-auto">
      <li
        class="ml-3 px-auto py-5 rounded-lg bg-blue-400 shadow-xl"
        v-for="(forecast, index) in mappedWeatherDataForecasts"
        :key="index"
      >
        <div class="flex flex-col justify-evenly items-center text-slate-100 w-16 h-32 mx-5 ">
          <div>{{ forecast.time }}</div>
          <div class="text-2xl font-semibold">{{ forecast.temperature_2m }}°C</div>
          <div>{{ forecast.weather_code }}</div>
        </div>
      </li>
    </ul>
  </div>
</template>
