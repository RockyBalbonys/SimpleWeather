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
      <h1 class="text-4xl py-2 text-slate-300 font-semibold">Simple Weather</h1>
      <hr />
      <div v-if="loc">
        <h2 class="text-xl text-slate-400 font-normal">{{ loc.city }}, {{ loc.country }}</h2>
      </div>
    </header>
  </div>
</template>
