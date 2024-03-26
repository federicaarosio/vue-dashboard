<template>
    <main>
        <TopCharts />

        <div class="card-wrapper mb-2rem">
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

import LineChart from './LineChart.vue';

import TopCharts from './TopCharts.vue';


export default {
    components: {
        TopCharts,
        LineChart
    },

    data() {
        return {
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


        generateGaussianArray() {
            const nElements = 24;
            const mu = 12; // Media della gaussiana
            const sigma = 3; // Deviazione standard della gaussiana

            // Funzione per calcolare il valore della gaussiana in un dato punto x
            function gaussian(x) {
                return Math.exp(-(Math.pow(x - mu, 2) / (2 * Math.pow(sigma, 2))));
            }

            const result = [];

            // Genera i valori della gaussiana per ciascun indice
            for (let i = 0; i < nElements; i++) {
                const x = i;
                let value = gaussian(x);

                // Applica una variazione casuale tra -5% e +5%
                const variation = (Math.random() * 0.1) - 0.05;
                value += value * variation;

                // Normalizza il valore tra 0 e 95
                value *= 95 / 100;

                // Assicura che il valore non superi mai 95
                value = Math.min(value, 0.95);

                result.push(Math.round(value * 100));
            }

            return result;
        },

        generateSolarChart() {
            // Dati di prestazione solare e ore
            const standardPerformance = this.generateGaussianArray();
            const hours = Array.from({ length: 24 }, (_, i) => i); // Array di ore da 0 a 23

            this.solarPowerData.labels = hours;
            this.solarPowerData.datasets[0].data = standardPerformance;

            this.loadedSolarPowerData = true;
        },

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
</style>
