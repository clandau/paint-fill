<template>
  <div class="container">
    <div class="grid" :style="gridStyle">
      <div v-for="pixelRow in pixels" :key="pixelRow">
        <div
          v-for="pixel in pixelRow"
          :key="pixel"
          :class="'box box' + pixel"
        ></div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "Grid",
  data() {
    return {
      pixels: [
        [1, 1, 1],
        [1, 1, 0],
        [1, 0, 1],
      ],
      pixelSize: "100px"
    };
  },

  computed: {
    gridStyle() {
      const columns = this.pixels[0].length;
      return {
        display: "grid",
        "justify-content": "center",
        "align-content": "center",
        "grid-template-columns": `repeat(${columns}, ${this.pixelSize}`
      }
    }
  }
});
</script>

<style scoped>
.box {
  min-width: 25px;
  height: 100px;
  border-style: solid;
  border-width: 2px;
  border-color: navy;
}
.box0 {
  background-color: aquamarine;
}
.box1 {
  background-color: orangered;
}
</style>
