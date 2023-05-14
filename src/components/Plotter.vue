<script>
import Plotly from 'plotly.js-dist-min'

export default {
    name: "Plotter",
    props: {
        plotterData: {
            default: {
                x: [1, 2, 3, 4],
                y: [-10, -15, -13, -17],
                mode: "lines+markers",
                line: {shape: "spline"},
                type: "scatter"
            }
        }
    },
    data() {
        return {
            data: [{
                x: [1, 2, 3, 4],
                y: [10, 15, 13, 17],
                mode: "lines+markers",
                line: {shape: "spline"},
                type: "scatter"
            }],
            layout: {
                title: "My graph",
                xaxis: {
                    title: 'X'
                },
                yaxis: {
                    title: 'Y'
                },
                width: 1000,
                height: 1000,
                margin: {
                    l: 50,
                    r: 50,
                    b: 100,
                    t: 100,
                    pad: 4
                },
                paper_bgcolor: '#f0f0f0',
                plot_bgcolor: '#f0f0f0'
            },
            config: {
                scrollZoom: true,
                displaylogo: false,
                responsive: true,
                willReadFrequently: true
            }
        }
    },
    mounted() {
        this.buildGraph();
        window.addEventListener('resize', this.buildGraph);
    },
    beforeUnmount() {
        window.removeEventListener('resize', this.buildGraph);
    },
    methods: {
        buildGraph() {
            this.calculateGraphSize();
            Plotly.react(this.$refs.plotlyContainer,
                this.data,
                this.layout,
                this.config
            );
        },
        calculateGraphSize() {
            this.layout.width = this.$refs.graphBox.clientWidth - 10;
            this.layout.height = this.$refs.graphBox.clientHeight - 10;
        }

    },
    watch: {
        plotterData() {
            this.data[0] = this.plotterData;
            this.buildGraph();
        }
    }
}
</script>

<template>
    <div class="plotter" ref="graphBox">
        <div class="container" ref="plotlyContainer"/>
    </div>
</template>

<style scoped>
.plotter {
    width: 100%;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: auto;
    user-select: none;

    .container {
        max-width: 100%;
        max-height: 100%;
        overflow: hidden;
        border-radius: 20px;
    }
}
</style>
