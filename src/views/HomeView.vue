<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        @input="getReachResults"
        v-model="searchQuery"
        type="text"
        placeholder="Search for a city or state"
        class="bg-transparent py-2 px-1 w-full border-b focus:border-weather-primary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
      <ul
        class="absolute bg-weather-secondary py-2 px-1 text-white w-full shadow-md top-[55px]"
        v-if="queryResults"
      >
        <p class="text-2xl" v-if="queryError">Sorry,something is wrong,please try it again</p>
        <p class="text-2xl" v-if="!queryError && queryResults.length === 0">
          No results match your query,try a different term.
        </p>
        <template v-else>
          <li
            v-for="item in queryResults"
            :key="item.id"
            class="py-2 px-1 cursor-pointer"
            @click="previewCity(item)"
          >
            {{ item.name }}
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const router = useRouter()
const weatherKey = 'SG1AzKWIAHdgmDzqZ'
const searchQuery = ref('')
const queryTimeout = ref(null)
const queryResults = ref(null)
const queryError = ref(false)

const getReachResults = () => {
  clearTimeout(queryTimeout.value)
  queryTimeout.value = setTimeout(async () => {
    // console.log(searchQuery.value)
    if (searchQuery.value != '') {
      try {
        const rep = await axios.get(
          // `https://api.seniverse.com/v3/weather/now.json?key=${weatherKey}&location=${searchQuery.value}&language=zh-Hans&unit=c`
          `https://api.seniverse.com/v3/location/search.json?key=${weatherKey}&q=${searchQuery.value}&limit=20&offset=10`
        )
        // console.log(rep.data.results)
        queryResults.value = rep.data.results
        return
      } catch {
        queryError.value = true
      }
    }

    queryResults.value = null
  }, 1000)
}

const previewCity = (searchResult) => {
  const [quyu, city] = searchResult.path.split(',')
  console.log(searchResult)
  console.log(quyu, city)
  // router.push({ name: 'cityView', params: { quyu: quyu, city: city } })
  router.push({ name: 'cityView', query: { preview: false } })
}
</script>
