<script>

import BaseCard from './layout/BaseCard.vue';
import BaseInfo from './layout/BaseInfo.vue';

export default {
    components: {
        BaseCard,
        BaseInfo
    },

    props: {
        main: {
            type: Object,
            required: true
        },
        wind: {
            type: Object,
            required: true
        },
        clouds: {
            type: Object,
            required: true
        },
        dt_txt: {
            type: String,
            required: true
        },
        fTheme: {
            type: Boolean,
            required: true
        }
    },

    emits: ['toggle-weather-info', 'delete-weather-card'],

    computed: {
        getWindDirection() {
            let wd;
            if (this.wind.deg > 337.5 && this.wind.deg <= 22.5) wd = 'Пн';
            else if (this.wind.deg > 22.5 && this.wind.deg <= 67.5) wd = 'Пн-Сх';
            else if (this.wind.deg > 67.5 && this.wind.deg <= 112.5) wd = 'Сх';
            else if (this.wind.deg > 112.5 && this.wind.deg <= 157.5) wd = 'Пд-Сх';
            else if (this.wind.deg > 157.5 && this.wind.deg <= 202.5) wd = 'Пд';
            else if (this.wind.deg > 202.5 && this.wind.deg <= 247.5) wd = 'Пд-Зх';
            else if (this.wind.deg > 247.5 && this.wind.deg <= 292.5) wd = 'Зх';
            else wd = 'Пн-Зх';
            return wd;
        },
        getTemp() {
            return Math.round(this.main.feels_like);
        },
        getPressure() {
            return Math.round(this.main.pressure / 133.4 * 100);
        }
    },

    methods: {
        closeInfo() {
            this.$emit('toggle-weather-info');
        },
        deleteCard() {
            this.$emit('delete-weather-card');
        }
    }
}

</script>

<template>
    <BaseCard class="absolute w-full h-3/5 left-0 bottom-0 backdrop-blur-[6px] bg-white/70 p-4 z-10 text-gray-600
    flex flex-col gap-2">
        <div class="self-end">
            <button @click="closeInfo" :class="{ 'hover:text-[#2a94fe]': fTheme, 'hover:text-[#190b6a]': !fTheme }"
                class="transition-all ease duration-300"><i class="fa-solid fa-xmark"></i></button>
        </div>
        <div class="grid grid-cols-3 gap-x-2 gap-y-2 flex-[1_1_100%] items-center">

            <BaseInfo>
                <template #img><i class="fa-solid fa-temperature-three-quarters"></i></template>
                <span>{{ getTemp }} &deg;C</span>
                <template #description><span>Відчувається</span></template>
            </BaseInfo>

            <BaseInfo>
                <template #img><i class="fa-solid fa-wind"></i></template>
                <span>{{ wind.speed }} м/с</span>
                <template #description><span>{{ getWindDirection }}</span></template>
            </BaseInfo>

            <BaseInfo>
                <template #img><i class="fa-solid fa-droplet"></i></template>
                <span>{{ main.humidity }} %</span>
                <template #description><span>Вологість</span></template>
            </BaseInfo>

            <BaseInfo>
                <template #img><i class="fa-solid fa-arrows-down-to-line"></i></template>
                <span>{{ getPressure }} мм</span>
                <template #description><span>Тиск</span></template>
            </BaseInfo>

            <BaseInfo>
                <template #img><i class="fa-solid fa-cloud-sun-rain"></i></template>
                <span>{{ clouds.all }} %</span>
                <template #description><span>Хмарність</span></template>
            </BaseInfo>

        </div>
        <div class="flex justify-between items-center gap-1">
            <span class="text-xs">Оновлено {{ dt_txt }}</span>
            <button @click="deleteCard" :class="{ 'hover:text-[#2a94fe]': fTheme, 'hover:text-[#190b6a]': !fTheme }"
                class="transition-all ease duration-300"><i class="fa-solid fa-square-minus"></i></button>
        </div>
    </BaseCard>
</template>