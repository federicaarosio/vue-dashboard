<template lang="">
    <div>
        <div class="card-wrapper mb-2rem">
            <h2>Monthly Connections</h2>
            <LineChart class="custom-line" :chartData="monthlyConnectionsData" :chartOptions="monthlyConnectionsOptions"
                :loaded="loadedmonthlyConnectionsData" />
        </div>
        <div class="diagrams-wrapper mb-2rem">
            <div class="card-wrapper col-50">
                <h2>Users Age Range</h2>
                <BarChart class="custom-bar" :chartData="usersAgeRangesData" :chartOptions="usersAgeRangesOptions"
                    :loaded="loadedAgeRangeData" />
            </div>
            <div class="card-wrapper col-50">
                <h2>Devices</h2>
                <DoughnutChart class="custom-doughnut" :chartData="devicesData" :chartOptions="devicesOptions"
                    :loaded="loadedDevicesData" />
            </div>
        </div>
        <hr class="mb-2rem">
    </div>
</template>
<script>
import axios from 'axios';
import BarChart from './BarChart.vue';
import DoughnutChart from './DoughnutChart.vue';
import LineChart from './LineChart.vue';
export default {
    name: 'TopCharts',
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
                        label: 'Monthly',
                        backgroundColor: ['#8c8ec7'],
                        data: []
                    }
                ]
            },

            monthlyConnectionsOptions: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    title: {
                        display: true,
                        text: 'Monthly Connections'
                    }
                },
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
                        backgroundColor: ['#8c8ec7', '#97D9E1', '#D9AFD9', '#f5a960', '#eb99a5'],
                        data: []
                    }
                ]
            },

            usersAgeRangesOptions: {
                responsive: true,
                maintainAspectRatio: false,
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
                },

            },


            // Dati per il terzo grafico: Devices
            loadedDevicesData: false,

            devicesData: {
                labels: [],
                datasets: [
                    {
                        label: 'Operating System',
                        backgroundColor: ['#8c8ec7', '#97D9E1', '#D9AFD9', '#f5a960', '#eb99a5'],
                        data: []
                    }
                ]
            },

            devicesOptions: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: {
                        display: true,
                        position: 'right'
                    }
                },
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
@use "../styles/partials/variables" as *;
@use "../styles/partials/mixins" as *;

    .card-wrapper {
    /* From https://css.glass */
    background: rgba(255, 255, 255, 0.495);
    border-radius: 16px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(5px);
    -webkit-backdrop-filter: blur(5px);
    border: 1px solid rgba(255, 255, 255, 0.31);
    padding: 2rem
}

.mb-2rem {
    margin-bottom: 2rem;
}

.custom-line {
    height: 270px;
}

.diagrams-wrapper {
    @include flex(row, start, center);
    gap: 2rem;

    .col-50 {
        width: 50%;
    }

    .custom-bar {
        height: 270px;
    }

    .custom-doughnut {
        height: 270px;
    }

}
</style>