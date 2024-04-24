<script>

export default {

    props: {
        wDate: {
            type: Number,
            required: true
        },
        wIcon: {
            type: String,
            required: true
        },
        wDescription: {
            type: String,
            required: true
        },
        wTemp: {
            type: Number,
            required: true
        },
        fFrc: {
            type: Boolean,
            required: true
        }
    },

    computed: {
        getTime() {
            return `${new Date(this.wDate * 1000).getHours()}:00`;
        },
        getIcon() {
            return `https://openweathermap.org/img/wn/${this.wIcon}${this.fFrc ? '' : '@4x'}.png`;
        },
        getDescription() {
            const str = this.wDescription;
            return str.charAt(0).toUpperCase() + str.slice(1);
        },
        getTemp() {
            return Math.round(this.wTemp);
        }
    }
}

</script>

<template>
    <div>
        <div v-if="this.fFrc">
            {{ getTime }}
        </div>
        <div :class="{ 'flex-[0_0_auto]': !this.fFrc }">
            <img :src="getIcon" :alt="getDescription">
        </div>
        <div :class="{ 'flex flex-col gap-1 flex-[1_1_100%]': !this.fFrc }">
            <span :class="{ 'text-4xl': !this.fFrc }">
                {{ getTemp }}&deg;C
            </span>
            <span v-if="!this.fFrc">
                {{ getDescription }}
            </span>
        </div>
    </div>
</template>

<style scoped></style>