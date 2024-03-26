<template>
    <main>
        <TopCharts />

        <div class="card-wrapper mb-2rem">
            <h3>Solar Power</h3>
            <p>
                Questa card visualizza i dati di produzione giornaliera dell'impianto fotovoltaico del
                nostro ufficio. Sebbene l'impianto non sia ancora operativo, è possibile esplorare un mockup dei dati
                generati ogni ora.
            </p>
            <!-- <p>
                Cliccando il pulsante "play", viene avviata la simulazione della produzione giornaliera dell'impianto
                fotovoltaico. Ogni secondo, viene aggiunto un valore preso dall'array dei rendimenti, rappresentando il
                rendimento dell'impianto in quella specifica ora del giorno. Inoltre, viene applicata una variazione
                casuale del ±5% per garantire una varietà nei dati simulati. I valori aggiornati vengono quindi
                visualizzati sul grafico in tempo reale.
            </p>
            <div class="text-center">
                <button type="button" class="btn btn-light rounded-4" @click="startSimulation">Play</button>
            </div> -->

            <LineChart class="custom-line" :chartData="chartData" :chartOptions="solarPowerOptions"
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
            loadedSolarPowerData: false,
            solarPowerData: [],
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

            interval: null
        };
    },

    computed: {
        chartData() {
            return {
                labels: this.solarPowerData.map((_, index) => index),
                datasets: [
                    {
                        label: 'Solar Power',
                        backgroundColor: '#f5a960',
                        data: this.solarPowerData
                        // data: this.solarPowerData.map(performance => ({ y: performance }))
                    }
                ]
            };
        }
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
                let value = gaussian(i);

                // Applica una variazione casuale tra -10% e +10%
                const variation = (Math.random() * 0.2) - 0.1;
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
            this.solarPowerData = this.generateGaussianArray();
            this.loadedSolarPowerData = true;
        },

        // Funzione per avviare l'aggiornamento del grafico
        startSimulation() {
            // Svuota l'array solarPowerData prima di iniziare la simulazione
            console.log(this.solarPowerData)
            this.solarPowerData = [];
            // Array contenente i rendimenti per ogni ora del giorno
            const solarPerformances = this.generateGaussianArray();
            let index = 0;

            // Intervallo che esegue l'azione ogni secondo
            this.interval = setInterval(() => {
                // Controlla se ci sono ancora valori da inviare al grafico
                if (index < solarPerformances.length) {
                    // Ottieni il rendimento per l'ora corrente
                    const performance = solarPerformances[index];
                    console.log(`ora: ${index}, rendimento: ${performance}`)

                    // // Aggiorna il valore del grafico
                    this.updateChart(performance);

                    // Incrementa l'indice per ottenere il rendimento per l'ora successiva
                    index++;

                } else {
                    // Se hai inviato tutti i valori, ferma l'intervallo
                    clearInterval(this.interval);
                    console.log(this.solarPowerData)

                }
            }, 1000); // Intervallo di 1 secondo
        },

        updateChart(performance) {
            // Aggiorna i dati del grafico con il nuovo valore
            // this.solarPowerData.push({ y: performance });
            this.solarPowerData.push(performance);
        }
    },

    mounted() {
        this.generateSolarChart();
    }
};
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
