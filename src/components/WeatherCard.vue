<script>

import BaseCard from './layout/BaseCard.vue';
import BaseLine from './layout/BaseLine.vue';
import WeatherItem from './WeatherItem.vue';
import WeatherForecast from './WeatherForecast.vue';
import WeatherInfo from './WeatherInfo.vue';

export default {

  components: {
    BaseCard,
    BaseLine,
    WeatherItem,
    WeatherForecast,
    WeatherInfo
  },

  props: {
    place: {
      type: Object,
      required: true
    },
    forecast: {
      type: Array,
      required: true
    }
  },

  emits: ['delete-weather-card'],

  data() {
    return {
      dt: null,
      clock: { "h": 0, "m": 0 },
      infoOpen: false,
      lTime: ''
    }
  },

  computed: {
    getLocalTime() {
      return `${this.clock.h}:${this.clock.m.toString().padStart(2, '0')}`;
    },
    getTheme() {
      return (this.clock.h >= 6 && this.clock.h < 19) ? true : false;
    }
  },

  methods: {
    getTime(inc) {
      if (inc === 0) {
        const d = new Date();
        const localTime = d.getTime();
        const localOffset = d.getTimezoneOffset() * 60000;
        const utc = localTime + localOffset;
        const offset = this.place.timezone / 3600;
        this.dt = utc + (3600000 * offset);
      }
      this.dt += inc;
      this.clock.h = new Date(this.dt).getHours();
      this.clock.m = new Date(this.dt).getMinutes();

    },
    toggleWeatherInfo() {
      this.infoOpen = !this.infoOpen;
    },
    deleteWeatherCard() {
      this.infoOpen = !this.infoOpen;
      this.$emit('delete-weather-card');
    }
  },

  created() {
    this.getTime(0);
    setInterval(() => this.getTime(1000), 1000);
  }

}

</script>

<template>
  <BaseCard :class="getTheme ? 'bg-day' : 'bg-night'" class="relative text-white p-4 flex flex-col gap-1" >

    <div class="flex justify-between align-center gap-1 text-lg">
      <div class="flex items-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        {{ this.place.local_name }}, {{ this.place.sys.country }}
      </div>
      <div class="flex items-center gap-2">
        <i class="fa-solid fa-clock"></i>
        {{ this.getLocalTime }}
      </div>
    </div>

    <WeatherItem :wDate="this.place.dt" :wIcon="this.place.weather[0].icon"
      :wDescription="this.place.weather[0].description" :wTemp="this.place.main.temp" :fFrc="false"
      class="flex justify-between items-center flex-auto gap-2"></WeatherItem>

    <WeatherForecast :forecast="this.forecast"></WeatherForecast>

    <BaseLine class="my-8"></BaseLine>

    <div class="flex justify-end items-center gap-2">
      <a href="" @click.prevent="toggleWeatherInfo" :class="{ 'text-[#c1defa]': getTheme, 'text-[#5b86b0]': !getTheme }"
        class="hover:text-white transition-all ease duration-300">
        Детальніше <i class="fa-solid fa-arrow-right text-sm"></i></a>
    </div>

    <Transition name="fade">
      <WeatherInfo v-show="infoOpen" @toggle-weather-info="toggleWeatherInfo" @delete-weather-card="deleteWeatherCard"
        :main="this.place.main" :wind="this.place.wind" :clouds="this.place.clouds" :dt_txt="this.place.dt_txt"
        :fTheme="this.getTheme">
      </WeatherInfo>
    </Transition>

  </BaseCard>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.bg-day {
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #2a94fe 100%);
}

.bg-night {
  background-color: #190b6a;
  background-image: linear-gradient(62deg, #335f8a 0%, #190b6a 100%);
}
</style>