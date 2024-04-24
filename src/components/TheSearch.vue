<script>
import BaseCard from './layout/BaseCard.vue'

export default {

    components: {
        BaseCard
    },

    props: {
        ApiKeyWeather: {
            type: String,
            required: true
        }
    },

    emits: ['addCity'],

    data() {
        return {
            city: '',
            cities: [],
            timeout: null,
        }
    },

    methods: {
        searchCity() {
            clearTimeout(this.timeout);

            if (this.city.length > 2) {

                this.timeout = setTimeout(async () => {
                    fetch(`...`)
                        .then((response) => {
                            if (response.ok) {
                                return response.json();
                            }
                        })
                        .then((data) => {
                            this.cities = data;
                        })
                }, 500);

            }
            else {
                this.cities = [];
            }
        },

        addCity(lat, lon, local_name) {
            this.$emit('addCity', { "lat": lat, "lon": lon, "local_name": local_name });
            this.city = '';
            this.cities = [];
        }
    }
}

</script>

<template>
    <div class="relative">
        <form>

            <BaseCard class="flex items-center">
                <i class="absolute fa-solid fa-magnifying-glass p-2 text-sky-600"></i>
                <input type="search" name="city" id="city" placeholder="Пошук міста" autocomplete="off"
                    class="rounded-lg p-2 pl-10 border-0 outline-0 focus:ring-2 focus:ring-sky-600 w-full transition-all ease duration-300"
                    v-model="city" @input.keyup="searchCity">
            </BaseCard>

        </form>

        <div v-if="cities" class="absolute z-10 w-full">

            <BaseCard class="flex-col bg-white/90 my-4">
                <button v-for="(city, i) in cities" :key="i" @click="addCity(city.lat, city.lon, city.local_names.uk)"
                    class="px-3 my-2 hover:text-sky-600 w-full text-left transition-all ease duration-300">
                    {{ city.name }}<span v-show="city.hasOwnProperty('state')"> {{ ', ' + city.state }}</span>, {{
                        city.country }}
                </button>
            </BaseCard>

        </div>
    </div>
</template>