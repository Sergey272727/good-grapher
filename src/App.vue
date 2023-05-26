<script>
import Plotter from "@/components/Plotter.vue";
import DropDown from "@/components/DropDown.vue";
import CheckBoxList from "@/components/CheckBoxList.vue";

export default {
    name: "App",
    components: {CheckBoxList, Plotter, DropDown},
    data() {
        return {
            points: [],
            graphType: '',
            graphDim: '2D',
            checkedPlots: [],
            fileNames: []
        }
    },
    methods: {
        changeGraphTypeHandler(graphType) {
            this.graphType = graphType;
        },
        changeGraphDimHandler(graphDim) {
            this.graphDim = graphDim;
            this.$refs.graphTypeDropDown.selectedOption = this.$refs.graphTypeDropDown.options[0];
        },
        changePlotCheckBoxHandler(checkedPlots) {
            this.checkedPlots = checkedPlots;
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

            const width = [...new Set(obj.x)].length;
            const height = [...new Set(obj.y)].length;
            const dx = (maxX - minX) / obj.x.length;
            const dy = (maxY - minY) / obj.y.length;

            const matrix = new Array(height).fill(null).map(() => new Array(width).fill(null));

            for (let i = 0; i < obj.x.length; i++) {
                let x = Math.floor((obj.x[i] - minX) / dx / height);
                let y = Math.floor((obj.y[i] - minY) / dy / width);
                x = x >= width ? width - 1 : x;
                y = y >= height ? height - 1 : y;
                matrix[y][x] = obj.z[i];
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
                    if (!pointsArray[pointsArray.length - 1][0]) {
                        pointsArray.pop();
                    }
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
                    dataArray.name = pointInFile.name;
                    this.points.push(dataArray);
                    this.fileNames.push(pointInFile.name);
                }
            }
            this.$refs['file-input'].files = null;
        },
    },
    computed: {
        plotterData() {
            let plotsArray = [];
            this.points.forEach(value => {
                let go = false;
                this.checkedPlots.forEach(plot => {
                    if (value.name === plot) {
                        go = true;
                    }
                });
                if (go) {
                    let data = {};
                    if (this.graphDim === '2D') {
                        data.x = value.x;
                        data.y = value.y;
                    } else if (this.graphDim === '3D') {
                        data.x = value.x;
                        data.y = value.y;
                        data.z = value.z;
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
                            data.z = this.convertToMatrix(data);
                            data.x = [...new Set(data.x)];
                            data.y = [...new Set(data.y)];
                            data.type = 'heatmap';
                            break;
                        }
                        case 'surface': {
                            data.z = this.convertToMatrix(data);
                            data.x = [...new Set(data.x)];
                            data.y = [...new Set(data.y)];
                            data.type = 'surface';
                            break;
                        }
                        default: {
                            data.mode = "lines+markers";
                            data.line = {shape: "spline"};
                            data.type = "scatter";
                        }
                    }
                    plotsArray.push(data);
                }
            });
            return plotsArray;
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
                        {key: 'mesh3d', name: 'Объектный график'},
                        {key: 'line3d', name: 'Линейный график'},
                        {key: 'hm', name: 'Тепловая карта'},
                        {key: 'surface', name: 'График поверхности'}
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
    }
    ,
    watch: {
        graphType() {
            switch (this.graphType) {
                case 'scatter': {
                    this.$refs.exampleImage.src = 'src/assets/img/line-and-scatter.jpg';
                    break;
                }
                case 'line': {
                    this.$refs.exampleImage.src = 'src/assets/img/line-plots.jpg';
                    break;
                }
                case 'bar': {
                    this.$refs.exampleImage.src = 'src/assets/img/bar.jpg';
                    break;
                }
                case "spline": {
                    this.$refs.exampleImage.src = 'src/assets/img/error-cont.jpg';
                    break;
                }
                case "scatter3d": {
                    this.$refs.exampleImage.src = 'src/assets/img/3d-scatter.jpg';
                    break;
                }
                case "countour": {
                    this.$refs.exampleImage.src = 'src/assets/img/contour.jpg';
                    break;
                }
                case "mesh3d": {
                    this.$refs.exampleImage.src = 'src/assets/img/3d-mesh.jpg';
                    break;
                }
                case "line3d": {
                    this.$refs.exampleImage.src = 'src/assets/img/3d-line.jpg';
                    break;
                }
                case 'hm': {
                    this.$refs.exampleImage.src = 'src/assets/img/heatmap.jpg';
                    break;
                }
                case 'surface': {
                    this.$refs.exampleImage.src = 'src/assets/img/3d-surface.jpg';
                    break;
                }
                default: {
                    this.$refs.exampleImage.src = 'src/assets/img/error-cont.jpg';
                }
            }
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
                            <check-box-list :checkBoxData="fileNames" @change-option="changePlotCheckBoxHandler"></check-box-list>
                        </div>
                    </div>
                    <div class="toolbar__lower">
                        <drop-down
                                class="dropdown-button"
                                element-name="Размерность"
                                :options="[
                                {key: '2D', name: 'Двумерный график'},
                                {key: '3D', name: 'Трехмерный график'},
                            ]"
                                @change-option="changeGraphDimHandler"
                        ></drop-down>
                        <drop-down
                                ref="graphTypeDropDown"
                                element-name="Тип графика"
                                :options="graphTypeOptions"
                                @change-option="changeGraphTypeHandler"
                        ></drop-down>
                        <div class="toolbar__lower-example">
                            <div class="title">Пример графика</div>
                            <img ref="exampleImage" class="graph-example__image" src="src/assets/img/line-plots.jpg"
                                 alt="preview">
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
    font-size: 1.75rem;
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
    font-size: 2rem;
    border-radius: 1.25rem;
}

.graph-example__image {
    padding: 1.25rem;
    object-fit: cover;
    height: 100%;
    width: 100%;
    border-radius: 1.25rem;
}

.dropdown-button {
    margin-bottom: 3rem;
}
</style>
