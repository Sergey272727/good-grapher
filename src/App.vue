<script>


import Plotter from "@/components/Plotter.vue";

export default {
    name: "App",
    components: {Plotter},
    data() {
        return {
            points: [],
            graphType: [],
            file: '',

        }
    },
    methods: {
        simulateClickOnInput() {
            this.$refs['file-input'].click();
        },
        simulateClickOnSelect(){
            this.$refs.graph_type_select;
        },
        onInputChange() {
            const reader = new FileReader();
            const pointInFile = this.$refs['file-input'].files[0];
            if (pointInFile) {
                reader.readAsText(pointInFile);
                reader.onload = () => {
                    this.file = reader.result
                    let pointsArray = this.file.split('\r\n');
                    pointsArray = pointsArray.map(stroke => stroke.split(',').map(str => +str));
                    console.log(pointsArray);
                }
            }
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
                <plotter></plotter>
            </div>
            <aside class="toolbar">
                <div class="toolbar_container">
                    <div class="toolbar__label">
                        <span>Меню</span>
                    </div>
                    <div class="toolbar__upper">
                        <div class="toolbar__button" @click="simulateClickOnInput">
                            <div class="toolbar__data-button-text">
                                <span>Данные</span>
                            </div>
                            <input @change="onInputChange()" type="file" class="file-input" ref="file-input">
                        </div>
                        <div class="toolbar__upper-text">
                            {{ file }}
                        </div>
                    </div>
                    <div class="toolbar__lower">
                        <div class="toolbar__button" @click="simulateClickOnSelect">
                            <div class="toolbar__graph-type-button-text">
                                <select class="graph-type-select" ref="graph_type_select" >
                                    <option class="graph-type-option" value="">Тип графика</option>
                                    <option class="graph-type-option" value="2d">2D</option>
                                    <option class="graph-type-option" value="3d">3D</option>
                                    <option class="graph-type-option" value="4d">4D</option>
                                </select>
                            </div>
                        </div>
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
    font-size: 2rem;
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

.toolbar__graph-type-button-text {
    display: flex;
}

.toolbar__graph-type-button-text::before {
    padding-right: 1.5rem;
    padding-left: 1rem;
    display: inline-block;
    transform: rotate(90deg);
    content: url("assets/img/arrow.svg");
}

.toolbar__lower {
    display: flex;
    flex-direction: column;
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

.graph-type-select{
    all: unset;
    background-color: inherit;
    color: #fff;
    font-size: 2rem;
    font-family: inherit;
    border-radius: 20px;
}
.graph-type-option {
    all: unset;
    background-color: #00A537;
    border: none;
    color: #fff;
    border-radius: 20px;
}
.graph-type-option::before {
    padding-right: 1.5rem;
    padding-left: 1rem;
    content: url("assets/img/arrow.svg");
}
.graph-type-option:hover {
    background-color: #6BAE7A;
}
</style>
