<script>


import Plotter from "@/components/Plotter.vue";
import DropDown from "@/components/DropDown.vue";
import {NSelect} from "naive-ui"

export default {
    name: "App",
    components: {Plotter, DropDown, NSelect},
    data() {
        return {
            points: {
                x: [1, 2, 3, 4],
                y: [-10, -15, -13, -17]
            },
            pointsArray: [],
            graphType: '',
            graphDim: '2D',
            file: ''
        }
    },
    methods: {
        changeGraphTypeHandler(graphType) {
            this.graphType = graphType;
        },
        changeGraphDimHandler(graphDim) {
            this.graphDim = graphDim;
        },
        simulateClickOnInput() {
            this.$refs['file-input'].click();
        },
        onInputChange() {
            this.read3D();
        },
        convertToMatrix(obj) {
            const minX = Math.min(...obj.x);
            const minY = Math.min(...obj.y);
            const maxX = Math.max(...obj.x);
            const maxY = Math.max(...obj.y);

            const width = [... new Set(obj.x)].length;
            const height = [... new Set(obj.y)].length;
            const dx = (maxX - minX) / obj.x.length;
            const dy = (maxY - minY) / obj.y.length;

            const matrix = new Array(height).fill(null).map(() => new Array(width).fill(0));

            for (let i = 0; i < obj.x.length; i++) {
                let x = Math.floor((obj.x[i] - minX)/dx/height);
                let y = Math.floor((obj.y[i] - minY)/dy/width);
                x = x >= width ? width - 1 : x;
                y = y >= height ? height - 1 : y;
                const z = obj.z[i];
                matrix[y][x] = z;
            }

            return matrix;
        },
        read3D() {
            const reader = new FileReader();
            const pointInFile = this.$refs['file-input'].files[0];
            const mainDelimiter = '\r';
            const subDelimiter = ' ';
            const regExp = new RegExp(/\D /, 'g');
            let file = '';
            if (pointInFile) {
                reader.readAsText(pointInFile);
                reader.onload = () => {
                    file = reader.result
                    file = file.replaceAll(regExp, ' ');
                    file = file.replaceAll(',', '.');
                    let pointsArray = file.split(mainDelimiter);
                    pointsArray = pointsArray.map(stroke => stroke.split(subDelimiter).map(str => +str));
                    this.pointsArray = pointsArray;
                    let dataArray = {
                        x: [],
                        y: [],
                        z: [],
                    };
                    pointsArray.forEach(value => {
                        value[0] ? dataArray.x.push(value[0]) : dataArray.x.push(0);
                        value[1] ? dataArray.y.push(value[1]) : dataArray.y.push(0);
                        value[2] ? dataArray.z.push(value[2]) : dataArray.z.push(0);
                    })
                    this.points = dataArray;
                }
            }
        },
    },
    computed: {
        plotterData() {
            let data = {};
            if (this.graphDim === '2D') {
                data.x = this.points.x;
                data.y = this.points.y;
            } else if (this.graphDim === '3D') {
                data.x = this.points.x;
                data.y = this.points.y;
                data.z = this.points.z;
            }
            switch (this.graphType) {
                case 'scatter': {
                    data.mode = "markers";
                    data.type = "scatter";
                    data.marker = {size: 12};
                    break;
                }
                case 'line': {
                    data.mode = "lines+markers";
                    data.type = "scatter";
                    break;
                }
                case 'bar': {
                    data.type = "bar";
                    break;
                }
                case "spline": {
                    data.mode = "lines+markers";
                    data.line = {shape: "spline"};
                    data.type = "scatter";
                    break;
                }
                case "scatter3d": {
                    data.mode = "markers";
                    data.marker = {
                        size: 12,
                        line: {
                            color: 'rgba(217, 217, 217, 0.14)',
                            width: 0.5
                        },
                        opacity: 0.8
                    };
                    data.type = 'scatter3d';
                    break;
                }
                case "countour": {
                    data.type = 'contour';
                    break;
                }
                case "mesh3d": {
                    data.type = 'mesh3d';
                    data.opacity = 0.8;
                    data.color = 'rgb(300,100,200)';
                    break;
                }
                case "line3d": {
                    data.type = 'scatter3d';
                    data.mode = 'lines';
                    data.opacity = 1;
                    data.line = {
                        width: 6,
                        reversescale: false
                    };
                    break;
                }
                case 'hm': {
                    const inputObj = { x: [0.5, 1.5, 2.3], y: [1.2, 2.8, 0.4], z: [10, 20, 30] };
                    data.z = this.convertToMatrix(data);
                    data.x = [... new Set(data.x)]
                    data.y = [... new Set(data.y)];
                    data.type = 'heatmap';
                    break;
                }
                case 'surface': {
                    data.z = this.convertToMatrix(data);
                    data.type = 'surface';
                    data.contours = {
                        z: {
                            show: true,
                            usecolormap: true,
                            highlightcolor: "#42f462",
                            project: {z: true}
                        }
                    };
                    break;
                }
                default: {
                    data.mode = "lines+markers";
                    data.line = {shape: "spline"};
                    data.type = "scatter";
                }
            }
            console.log(data);
            return data;
        },
        graphTypeOptions() {
            let options = [];
            switch (this.graphDim) {
                case '2D': {
                    options = [
                        {key: 'scatter', name: 'Точечный график'},
                        {key: 'line', name: 'Линейный график'},
                        {key: 'spline', name: 'Сплайн график'},
                        {key: 'bar', name: 'Столбчатый график'}
                    ];
                    break;
                }
                case '3D': {
                    options = [
                        {key: 'scatter3d', name: 'Точечный график'},
                        {key: 'countour', name: 'График изолиний'},
                        {key: 'mesh3d', name: 'График поверхности'},
                        {key: 'line3d', name: 'Линейный график'},
                        {key: 'hm', name: 'Тепловая карта'}
                    ];
                    break;
                }
                default: {
                    options = [
                        {key: 'scatter', name: 'Точечный график'},
                        {key: 'line', name: 'Линейный график'},
                        {key: 'spline', name: 'Сплайн график'},
                        {key: 'bar', name: 'Столбчатая график'}
                    ];
                }
            }
            return options;
        }
    },
    watch: {
        points() {

        }
    }
}
</script>

<template>
    <div class="wrapper">
        <div class="content">
            <div class="canvas">
                <plotter :plotterData="plotterData"></plotter>
            </div>
            <aside class="toolbar">
                <div class="toolbar_container">
                    <div class="toolbar__label">
                        <span>Меню</span>
                    </div>
                    <div class="toolbar__upper">
                        <button class="toolbar__button" @click="simulateClickOnInput">
                            <span class="toolbar__data-button-text">Данные</span>
                            <input @change="onInputChange()" type="file" class="file-input" ref="file-input">
                        </button>
                        <div class="toolbar__upper-text">
                            {{ file }}
                        </div>
                    </div>
                    <div class="toolbar__lower">
                        <drop-down
                                element-name="Размерность"
                                :options="[
                                    {key: '2D', name: 'Двумерный график'},
                                    {key: '3D', name: 'Трехмерный график'},
                                ]"
                                @change-option="changeGraphDimHandler"
                        ></drop-down>
                        <drop-down
                                element-name="Тип графика"
                                :options="graphTypeOptions"
                                @change-option="changeGraphTypeHandler"
                        ></drop-down>
                        <div class="toolbar__lower-example">
                            <img class="dasha" src="src/assets/img/dasha.jpg" alt="preview">
                        </div>
                    </div>
                </div>
            </aside>
        </div>
    </div>
</template>

<style scoped>

.file-input {
    display: none;
}

.wrapper {
    width: 100%;
    height: 100%;
    background-color: #F0F0F0;
    display: flex;
    flex-direction: row;
}

.content {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: row;

}

.canvas {
    background-color: #F0F0F0;
    width: 80%;
    border-radius: 1.25rem;
}

.toolbar {

    width: 20%;
    height: 100vh;
    background-color: white;
    border: 1px solid #D9D9D9;
    border-radius: 1.25rem 0 0 1.25rem;

}

.toolbar_container {
    display: flex;
    justify-content: space-between;
    flex-direction: column;
    width: 90%;
    margin: 1.5rem auto;
}

.toolbar__label {
    font-size: 2rem;
    line-height: 2.375rem;
    padding-bottom: 3.438rem;
}

.toolbar__upper {
    display: flex;
    flex-direction: column;
}

.toolbar__button {
    background-color: #00A537;
    max-width: 21.875rem;
    height: 5.25rem;
    border-radius: 1.25rem;
    display: flex;
    flex-direction: row;
    justify-content: left;
    align-items: center;
    color: #fff;
    font-size: 1.75rem;
    box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
    border: none;
    cursor: pointer;
}

.toolbar__button:hover {
    background-color: #2C934E;
}

.toolbar__button:active {
    background-color: #6BAE7A;
}

.toolbar__upper-text {
    padding-top: 2.25rem;
    padding-bottom: 3.125rem;
}

.toolbar__data-button-text::before {
    padding-right: 1.5rem;
    padding-left: 1rem;
    content: url("assets/img/arrow.svg");
}

.toolbar__lower {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.toolbar__button {
    border-radius: 1.25rem;
    display: flex;
    flex-direction: row;
    justify-content: left;
    align-items: center;
    background-color: #00A537;
}

.toolbar__lower-example {
    padding-top: 3.188rem;
    height: 100%;
    width: 100%;
    border-radius: 1.25rem;
}

.dasha {
    object-fit: cover;
    height: 100%;
    width: 100%;
    border-radius: 1.25rem;
}
</style>
