<template>
    <div class="container">
        <Bar v-if="isDataLoaded" :data="chartData" :options="chartOptions" />
    </div>
</template>

<script>
import { Bar } from 'vue-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

export default {
    name: 'BarChart',
    components: {
        Bar,
    },

    props: {
        chartData: {
            type: Object,
            required: true
        },
        chartOptions: {
            type: Object,
            required: true
        },
        loaded: {
            type: Boolean,
            required: true
        },
    },

    computed: {
        isDataLoaded() {
            return this.loaded && this.chartData && Object.keys(this.chartData).length > 0 &&
            this.chartOptions && Object.keys(this.chartOptions).length > 0;
        }
    }
};
</script>

<style lang="scss">
</style>