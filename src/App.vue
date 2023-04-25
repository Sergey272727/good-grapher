<script setup>
import DataSelectionBox from "@/components/DataSelectionBox.vue";
import PlotTypeBox from "@/components/PlotTypeBox.vue";
import PlotCanvas from "@/components/PlotCanvas.vue";
</script>

<template>
  <div class="wrapper">
      <div class="content">
          <div class="canvas">
              <PlotCanvas></PlotCanvas>
          </div>
          <aside class="toolbar">
              <div class="toolbar__label"></div>
              <div class="toolbar__upper">
                  <DataSelectionBox></DataSelectionBox>
              </div>
              <div class="toolbar__lower">
                  <PlotTypeBox></PlotTypeBox>
              </div>
          </aside>
      </div>
  </div>
</template>

<style scoped>
* {
    border: 1px solid red;
}
.wrapper {
    max-width:1000px;
    margin: 0 auto;
}
.content {
    display: flex;
    flex-direction: row;
}
.canvas{
    width: 80%;
    height: 100%;
}
.toolbar {
    width: 20%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}
.toolbar__label {
    width: 90%;
    height: 10%;
}
.toolbar__upper {
    width: 90%;
    height: 45%;
}
.toolbar__lower {
    width: 90%;
    height: 45%;
}


@media (min-width: 1024px) {

}
</style>
