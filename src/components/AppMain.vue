<template>
    <main>
        <div id="card-wrapper" class="mb-2rem">
            <h2>Title</h2>
            <LineChart class="custom-line" :chartData="monthlyConnectionsData" :chartOptions="monthlyConnectionsOptions"
                :loaded="loadedmonthlyConnectionsData" />
        </div>
        <div class="diagrams-wrapper mb-2rem">
            <div id="card-wrapper" class="col-50">
                <h2>Title</h2>
                <BarChart class="custom-bar" :chartData="usersAgeRangesData" :chartOptions="usersAgeRangesOptions"
                    :loaded="loadedAgeRangeData" />
            </div>
            <div id="card-wrapper" class="col-50">
                <h2>Title</h2>
                <DoughnutChart class="custom-doughnut" :chartData="devicesData" :chartOptions="devicesOptions"
                    :loaded="loadedDevicesData" />
            </div>
        </div>
        <hr class="mb-2rem">
        <div id="card-wrapper" class="mb-2rem">
            <h2>Solar Power</h2>
            <p>
                Questa card visualizza in tempo reale i dati di produzione giornaliera dell'impianto fotovoltaico del
                nostro ufficio. Sebbene l'impianto non sia ancora operativo, è possibile esplorare un mockup dei dati
                generati ogni ora.
            </p>
            <p>
                Cliccando il pulsante "play", viene avviata la simulazione della produzione giornaliera dell'impianto
                fotovoltaico. Ogni secondo, viene aggiunto un valore preso dall'array dei rendimenti, rappresentando il
                rendimento dell'impianto in quella specifica ora del giorno. Inoltre, viene applicata una variazione
                casuale del ±5% per garantire una varietà nei dati simulati. I valori aggiornati vengono quindi
                visualizzati sul grafico in tempo reale.
            </p>
            <div class="text-center">
                <button type="button" class="btn btn-light rounded-4" @click="generateSolarChart">Play</button>
            </div>

            <LineChart class="custom-line" :chartData="solarPowerData" :chartOptions="solarPowerOptions"
                :loaded="loadedSolarPowerData" />
        </div>
    </main>
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


            //Dati per performance pannelli solari
            interval: null,



            loadedSolarPowerData: false,

            solarPowerData: {
                labels: [],
                datasets: [
                    {
                        label: 'Solar Power',
                        backgroundColor: ['#8c8ec7'],
                        data: []
                    }
                ]
            },

            solarPowerOptions: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    title: {
                        display: true,
                        text: 'Solar Power'
                    }
                },
                scales: {
                    x: {
                        title: {
                            display: true,
                            text: 'Hours'
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: 'Performances'
                        }
                    }
                }
            },
        }
    },

    computed: {
        chartData() { return /* mutable chart data */ },
        chartOptions() { return /* mutable chart options */ }
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

        generateGaussianArray() {
            const nElements = 24;
            const mu = 12; // Media della gaussiana
            const sigma = 3; // Deviazione standard della gaussiana

            // Funzione per calcolare il valore della gaussiana in un dato punto x
            function gaussian(x) {
                return Math.exp(-(Math.pow(x - mu, 2) / (2 * Math.pow(sigma, 2))));
            }

            // Inizializza un array vuoto
            const result = [];

            // Genera i valori della gaussiana per ciascun indice
            for (let i = 0; i < nElements; i++) {
                // Calcola il valore x associato all'indice i
                const x = i;

                // Calcola il valore della gaussiana per x
                let value = gaussian(x);

                // Applica una variazione casuale tra -5% e +5%
                const variation = (Math.random() * 0.1) - 0.05;
                value += value * variation;

                // Normalizza il valore tra 0 e 95
                value *= 95 / 100;

                // Assicura che il valore non superi mai 95
                value = Math.min(value, 0.95);

                // Aggiungi il valore all'array risultante
                result.push(Math.round(value * 100));
            }

            // Ritorna l'array risultante
            return result;
        },

        generateSolarChart() {
            // Dati di prestazione solare e ore
            const standardPerformance = this.generateGaussianArray();
            const hours = Array.from({ length: 24 }, (_, i) => i); // Array di ore da 0 a 23
            
            // Aggiorna i dati del grafico solare
            this.solarPowerData.labels = hours;
            this.solarPowerData.datasets[0].data = standardPerformance;

            // Imposta il flag loadedSolarPowerData su true per indicare che i dati sono stati aggiornati
            this.loadedSolarPowerData = true;
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

main {
    height: calc(100vh - 4rem);
    overflow-y: scroll;
    padding: 0 2rem;
}

#card-wrapper {
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
