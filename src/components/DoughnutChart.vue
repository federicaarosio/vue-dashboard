<template>
    <div class="container">
        <Doughnut v-if="isDataLoaded" :data="chartData" :options="chartOptions" />
    </div>
</template>

<script>
import { Doughnut } from 'vue-chartjs';
import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js';

ChartJS.register(ArcElement, Tooltip, Legend);


export default {
    name: 'DoughnutChart',
    components: {
        Doughnut,
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