<template>
    <div id="card-wrapper">
        <!-- Primo grafico: Monthly Connections -->
        <LineChart :chartData="monthlyConnectionsData" :chartOptions="monthlyConnectionsOptions"
            :loaded="loadedmonthlyConnectionsData" />
    </div>

    <div id="card-wrapper">
        <!-- Secondo grafico: Users Age Ranges -->
        <BarChart :chartData="usersAgeRangesData" :chartOptions="usersAgeRangesOptions" :loaded="loadedAgeRangeData" />
    </div>

    <div id="card-wrapper">
        <!-- Terzo grafico: Devices -->
        <DoughnutChart :chartData="devicesData" :chartOptions="devicesOptions" :loaded="loadedDevicesData" />
    </div>
</template>

<script>
import axios from 'axios';

import BarChart from './BarChart.vue';
import DoughnutChart from './DoughnutChart.vue';
import LineChart from './LineChart.vue';


export default {
    components: {
        BarChart,
        DoughnutChart,
        LineChart
    },

    data() {
        return {

            // Dati per il primo grafico: Monthly Connections
            loadedmonthlyConnectionsData: false,

            monthlyConnectionsData: {
                labels: [],
                datasets: [
                    {
                        label: 'Monthly Connections',
                        backgroundColor: '#f87979',
                        data: []
                    }
                ]
            },

            monthlyConnectionsOptions: {
                responsive: true,
                scales: {
                    x: {
                        title: {
                            display: true,
                            text: 'Month'
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: 'Connections'
                        }
                    }
                }
            },


            // Dati per il secondo grafico: Users Age Ranges
            loadedAgeRangeData: false,

            usersAgeRangesData: {
                labels: [],
                datasets: [
                    {
                        label: 'Users Age Range',
                        backgroundColor: '#007bff',
                        data: []
                    }
                ]
            },

            usersAgeRangesOptions: {
                responsive: true,
                scales: {
                    x: {
                        title: {
                            display: true,
                            text: 'Age Range'
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: 'Connections'
                        }
                    }
                }
            },


            // Dati per il terzo grafico: Devices
            loadedDevicesData: false,

            devicesData: {
                labels: [],
                datasets: [
                    {
                        label: 'Operating System',
                        backgroundColor: '#007bff',
                        data: []
                    }
                ]
            },

            devicesOptions: {
                responsive: true,
                scales: {
                    x: {
                        title: {
                            display: true,
                            text: 'Device'
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: 'Connections'
                        }
                    }
                }
            },

        }
    },
    methods: {
        getMonthlyConnections() {
            axios.get('http://localhost:3000/MonthlyConnections')
                .then((response) => {
                    console.log(response.data);

                    const data = response.data;
                    this.monthlyConnectionsData.labels = data.map(item => item.month);
                    this.monthlyConnectionsData.datasets[0].data = data.map(item => item.connections);

                    this.loadedmonthlyConnectionsData = true;
                })
                .catch((error) => {
                    console.error('Errore durante il recupero dei dati:', error);
                });
        },

        getUsersAgeRanges() {
            axios.get('http://localhost:3000/UsersAgeRange')
                .then((response) => {
                    console.log(response.data);

                    const data = response.data;
                    this.usersAgeRangesData.labels = data.map(item => item.range);
                    this.usersAgeRangesData.datasets[0].data = data.map(item => item.connections);

                    this.loadedAgeRangeData = true;
                })
                .catch((error) => {
                    console.error('Errore durante il recupero dei dati:', error);
                });
        },

        getDevices() {
            axios.get('http://localhost:3000/Devices')
                .then((response) => {
                    console.log(response.data);

                    const data = response.data;
                    this.devicesData.labels = data.map(item => item.os);
                    this.devicesData.datasets[0].data = data.map(item => item.connections);

                    this.loadedDevicesData = true;
                })
                .catch((error) => {
                    console.error('Errore durante il recupero dei dati:', error);
                });
        },

    },
    mounted() {
        this.getMonthlyConnections();
        this.getUsersAgeRanges();
        this.getDevices();
    },
}
</script>

<style lang="scss" scoped>
#card-wrapper {
    /* From https://css.glass */
    background: rgba(255, 255, 255, 0.441);
    border-radius: 16px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(5px);
    -webkit-backdrop-filter: blur(5px);
    border: 1px solid rgba(255, 255, 255, 0.31);
    padding: 2rem
}
</style>
