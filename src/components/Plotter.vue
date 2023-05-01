<script>
import Plotly from 'plotly.js-dist-min'

export default {
    name: "Plotter",
    data() {
        return {
            data: [{
                x: [1, 2, 3, 4],
                y: [10, 15, 13, 17],
                type: "scatter"
            }],
            layout: {
                title: "My graph"
            }
        }
    },
    methods: {
        init() {
            Plotly.newPlot("plotly", /* JSON object */ {
                "data": [{"y": [1, 2, 3], "x": [1, 5, 10], "type": "scatter"}],
                "layout": {"width": 600, "height": 400}
            })
        }
    }
}
</script>

<template>
    <div @click="init()" class="plotly">
        <div class="container" id="plotly">
        </div>
    </div>
</template>

<style scoped>
.plotly {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: auto;
    user-select: none;
    background-color: #686c72;

    .container {
        width: 100%;
        height: 100%;
        overflow: hidden;
    }
}
</style>
