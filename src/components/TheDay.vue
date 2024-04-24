<script>

export default {

    props: {
        dt: {
            type: Date,
            required: true
        },
        prm: {
            type: String,
            required: true
        }
    },

    computed: {
        getWeekday() {
            const dt = this.dt.toLocaleDateString('uk-ua', {
                weekday: this.prm
            });
            return dt.charAt(0).toUpperCase() + dt.slice(1);
        },
        getDay() {
            return this.dt.toLocaleDateString('uk-ua', {
                year: 'numeric',
                month: this.prm,
                day: 'numeric'
            });
        },
    }
}

</script>

<template>

    <span>{{ getWeekday }}</span>, {{ getDay }}
    
</template>