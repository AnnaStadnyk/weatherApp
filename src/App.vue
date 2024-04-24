<script>
import TheDay from './components/TheDay.vue'
import TheSearch from './components/TheSearch.vue'
import WeatherCard from './components/WeatherCard.vue'

export default {
  components: {
    TheDay,
    TheSearch,
    WeatherCard
  },

  data() {
    return {
      currentDate: new Date(),
      ApiKeyWeather: '',
      cities: []
    }
  },

  computed: {
    getCnt() {
      return 8 - Math.floor(this.currentDate.getHours() / 3) - 1 + 16
    }
  },

  methods: {
    async addCity(coord) {
      try {
        const response = await fetch(`...`)
        const data = await response.json()
        const forecastsResponse = await fetch(`...`)
        const forecastsData = await forecastsResponse.json()

        let forecasts = []
        forecastsData.list.map((el, i) => {
          const forecast = {}
          forecast.dt = el.dt + forecastsData.city.timezone
          forecast.temp = el.main.temp
          forecast.icon = el.weather[0].icon
          forecast.description = el.weather[0].description
          forecast.date = new Date(forecast.dt * 1000).toLocaleDateString('uk-ua', {
            year: 'numeric',
            month: 'numeric',
            day: 'numeric'
          })
          forecasts.push(forecast)
        })
        data.forecast = forecasts

        data.local_name = coord.local_name
        data.dt_txt =
          new Date(data.dt * 1000).toLocaleString('uk-ua', {
            year: 'numeric',
            month: 'numeric',
            day: 'numeric'
            // hour: '2-digit',
            // minute: '2-digit',
            // second: '2-digit'
          }) +
          ' ' +
          new Date(data.dt * 1000).toLocaleTimeString('uk-ua')

        this.cities.push(data)
      } catch (error) {
        console.log('Request failed', error)
      }
    },

    deleteWeatherCard(id) {
      this.cities = this.cities.filter((c) => c.id !== id)
    }
  }
}
</script>

<template>
  <main>
    <div class="w-full xl:max-w-[1248px] px-4 xl:px-0 m-auto">
      <h2 class="text-center mb-6 text-2xl text-[#555] font-medium">
        <TheDay :dt="this.currentDate" prm="long"></TheDay>
      </h2>

      <TheSearch :ApiKeyWeather="ApiKeyWeather" @add-city="addCity"></TheSearch>

      <div
        class="grid max-[420px]:grid-cols-1 grid-cols-[repeat(auto-fill,320px)] lg:grid-cols-[repeat(auto-fill,360px)] gap-8 justify-center mt-8"
      >
        <WeatherCard
          v-for="(city, i) in cities"
          :key="i"
          :place="city"
          :forecast="city.forecast"
          @delete-weather-card="deleteWeatherCard(city.id)"
        ></WeatherCard>
      </div>
    </div>
  </main>
</template>

<style scoped></style>
