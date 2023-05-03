<script>


import Plotter from "@/components/Plotter.vue";
import DropDown from "@/components/DropDown.vue";
import { NSelect } from "naive-ui"
export default {
    name: "App",
    components: {Plotter, DropDown, NSelect},
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
            this.$refs.graph_type_select.up();
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
                        <button class="toolbar__button" @click="simulateClickOnInput">
                            <span class="toolbar__data-button-text">Данные</span>
                            <input @change="onInputChange()" type="file" class="file-input" ref="file-input">
                        </button>
                        <div class="toolbar__upper-text">
                            {{ file }}
                        </div>
                    </div>
                    <div class="toolbar__lower">
                        <drop-down></drop-down>
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
