<script>

import WeatherItem from './WeatherItem.vue';
import TheDay from './TheDay.vue';
import BaseLine from './layout/BaseLine.vue';

export default {
    components: {
        WeatherItem,
        TheDay,
        BaseLine
    },

    props: {
        forecast: {
            type: Array,
            required: true
        }
    },

    computed: {
        getUniqueDays() {
            let uniqueDays = [];
            this.forecast.map((el, i) => {
                let fUnique = true;
                uniqueDays.forEach(e => {
                    if (e.date === el.date) {
                        e.cnt++;
                        fUnique = false;
                    }
                })
                if (fUnique === true) {
                    uniqueDays.push({ 'dt': el.dt, 'date': el.date, 'cnt': 1 })
                }
            });
            return uniqueDays;
        }
    }
}

</script>

<template>
    <div class="overflow-x-auto pb-4">
    <table class="min-w-full text-center text-sm border-collapse">
        <thead>
            <tr>
                <th v-for="(day, i) in this.getUniqueDays" :key="i" :colspan="day.cnt" class="font-normal">

                    <TheDay v-if="day.cnt > 2" :dt="new Date(day.dt * 1000)" prm="short"></TheDay>
                    <span v-else-if="(day.cnt > 1) && (i === 0)">Сьогодні</span>
                    <span v-else class="inline-block"></span>

                    <BaseLine class="my-1"></BaseLine>

                </th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td v-for="(forecast, i) in this.forecast" :key="i">

                    <WeatherItem :wDate="forecast.dt" :wIcon="forecast.icon" :wDescription="forecast.description"
                        :wTemp="forecast.temp" :fFrc="true" class="w-10"></WeatherItem>

                </td>
            </tr>
        </tbody>
    </table>
</div>
</template>

<style scoped>
@media screen and (hover: hover) {
    ::-webkit-scrollbar {
        height: 8px;
        border-radius: 8px;
    }

    ::-webkit-scrollbar-track {
        background: #f1f1f1;
        border-radius: 8px;
    }

    ::-webkit-scrollbar-thumb {
        background: #555;
        border-radius: 8px;
    }
}
</style>