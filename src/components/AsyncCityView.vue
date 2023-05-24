<template>
  <div class="flex flex-col flex-1 items-center">
    <!-- Banner -->
    <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center">
      You are currently previewing this city,click the "+" icon to start tracking this city.
    </div>
    <!-- Weather overview -->
    <div class="flex flex-col flex-1 items-center py-12 text-white">
      <h1 class="text-4xl mb-2">{{ route.query.city }}</h1>
      <h2 class="text-sm mb-12">{{ curTime }}</h2>
      <p class="text-8xl mb-2">{{ weatherData[0].now.temperature }}&deg;</p>
      <h2 class="text-sm mb-2">{{ weatherData[0].now.text }}</h2>
    </div>
    <hr class="border border-white border-opacity-10 w-full" />
    <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <h2 class="mb-8">7天预报</h2>
        <div class="flex overflow-x-scroll gap-10 scrollbar-hide md:scrollbar-default">
          <div
            class="flex flex-col items-center gap-4"
            v-for="item in preWeathers"
            :key="item.date"
          >
            <h2 class="whitespace-nowrap">
              {{ moment(item.date).format('MM-DD') }}
            </h2>
            <h2 class="font-bold">{{ item.high }}</h2>
            <h2>{{ item.low }}</h2>
          </div>
        </div>
      </div>
    </div>
    <hr class="border border-white border-opacity-10 w-full" />
    <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <h2 class="mb-8">24小时预报</h2>
        <div class="flex flex-col gap-2">
          <div class="flex justify-between" v-for="item in preWeathers" :key="item.date">
            <!-- <h2>{{ item.date }}</h2> -->
            <h2 class="whitespace-nowrap">
              {{ moment(item.date).format('MM-DD') }}
            </h2>
            <div class="flex gap-8">
              <h2 class="font-bold">{{ item.high }}</h2>
              <h2 class="font">{{ item.low }}</h2>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from 'axios'
import moment from 'moment'
import { useRoute } from 'vue-router'
import { useNow, useDateFormat } from '@vueuse/core'
const weatherKey = 'SG1AzKWIAHdgmDzqZ'
const curTime = useDateFormat(useNow(), 'YYYY-MM-DD HH:mm')
const route = useRoute()
const getWeatherData = async () => {
  try {
    const rep = await axios.get(
      `https://api.seniverse.com/v3/weather/now.json?key=${weatherKey}&location=${route.query.city}&language=zh-Hans&unit=c`
    )
    return rep.data.results
  } catch (err) {
    console.log(err)
  }
}
const weatherData = await getWeatherData()
console.log(weatherData)

const getPreWeatherData = async () => {
  const rep = await axios.get(
    `https://api.seniverse.com/v3/weather/daily.json?key=${weatherKey}&location=${route.query.city}&language=zh-Hans&unit=c&start=0&days=5`
  )
  const array0 = rep.data.results[0].daily
  return array0.concat(array0).concat(array0)
}
const preWeathers = await getPreWeatherData()
</script>
