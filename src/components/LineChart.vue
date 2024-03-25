<template>
    <div class="container">
        <Line v-if="isDataLoaded" :data="chartData" :options="chartOptions" />
    </div>
</template>

<script>
import { Line } from 'vue-chartjs';
import { Chart as ChartJS, CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend } from 'chart.js';

ChartJS.register( CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend );


export default {
    name: 'LineChart',
    components: {
        Line,
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

<style lang="scss"></style>