<template>
  <div class="container">
    <h1>{{ msg }}</h1>
    <p>Click a box and be AMAZED at the color changes!</p>
    <p>Current color {{ currentColor }}</p>
    <div class="color-picker-wrapper">
      <div
        v-for="(color, i) in colorsArray"
        :class="['color-box', { 'box-border': isSelectedColor(i) }]"
        :style="`background-color: ${color};`"
        @click="handleColorSelect(i)"
        :id="`color-${i}`"
        :key="i"
      ></div>
    </div>
  </div>
  <div class="grid" :style="gridStyle">
    <div v-for="col of numCols" :key="`col-${col}`">
      <div
        v-for="row in numRows"
        :key="`row-${row}`"
        class="box"
        :id="`${row}-${col}`"
        @click="handleMouseClick"
      ></div>
    </div>
  </div>
  <div class="grid" :style="gridStyle" :class="{ 'rotate-center': isOneColor }">
    <div v-for="(pixelCol, i) in pixels" :key="i">
      <div
        v-for="(pixel, j) in pixelCol"
        :key="j"
        :class="['box box' + pixel]"
        :id="`${i}-${j}`"
        v-on:click="handleColorChange"
      ></div>
    </div>
  </div>

  <div class="container">
    <button class="button" @click="resetGrid">Reset Grid</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Grid",
  props: {
    msg: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      numCols: 20,
      numRows: 20,
      pixels: [
        [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0],
        [1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 1, 1],
        [1, 2, 2, 2, 2, 0, 1, 0, 1, 2, 2, 2, 2, 0, 1, 0],
        [1, 1, 1, 2, 2, 0, 1, 0, 1, 1, 1, 2, 2, 0, 1, 0],
        [1, 1, 1, 2, 2, 2, 2, 0, 1, 1, 1, 2, 2, 2, 2, 0],
        [1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1],
        [1, 1, 1, 1, 1, 2, 2, 1, 1, 1, 1, 1, 1, 2, 2, 1],
        [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
        [1, 0, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0],
        [1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0],
        [1, 2, 2, 2, 2, 0, 1, 0, 1, 2, 2, 2, 2, 0, 1, 0],
        [1, 1, 1, 2, 2, 0, 1, 0, 1, 1, 1, 2, 2, 0, 1, 0],
        [1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1],
        [1, 1, 1, 1, 1, 2, 2, 1, 1, 1, 1, 1, 1, 2, 2, 1],
        [1, 1, 1, 1, 1, 2, 2, 1, 1, 1, 1, 1, 1, 2, 2, 1],
      ],
      pixelSize: "30px",
      // colorsArray: ["#2c3e50", "#f34213", "#7fffd4", "#dda0dd"],
      colorsArray: [
        "#7366bd",
        "#47abcc",
        "#39a78e",
        "#93c572",
        "#f4c430",
        "#fa9618",
        "#ff6700",
        "#dc143c",
        "#f653a6",
        "#dda0dd",
      ],
      currentColor: "",
      fill: false,
    };
  },
  computed: {
    gridStyle(): object {
      return {
        display: "grid",
        "justify-content": "center",
        "align-content": "center",
        "grid-template-columns": `repeat(${this.numCols}, ${this.pixelSize})`,
      };
    },

    isOneColor() {
      const flattened: number[] = this.pixels.flat();
      return flattened.every((item) => item === flattened[0]);
    },
  },

  methods: {
    handleColorChange(e: MouseEvent) {
      const target = e.target as HTMLTextAreaElement;
      const [x, y] = target.id.split("-").map((i) => parseInt(i));
      this.paintFill(x, y, 3);
    },

    paintFill(x: number, y: number, newColor: number) {
      const currentVal = this.pixels[x][y];
      if (currentVal === newColor) return;
      // set currentVal to newColor
      this.pixels[x][y] = newColor;

      // check top, bottom, left and right
      // if they match currentVal, call function with that val's coordinates
      // top
      if (x - 1 >= 0 && this.pixels[x - 1][y] === currentVal) {
        this.paintFill(x - 1, y, newColor);
      }
      // bottom
      if (x + 1 < this.pixels.length && this.pixels[x + 1][y] === currentVal) {
        this.paintFill(x + 1, y, newColor);
      }
      // left
      if (y - 1 >= 0 && this.pixels[x][y - 1] === currentVal) {
        this.paintFill(x, y - 1, newColor);
      }
      // right
      if (
        y + 1 < this.pixels[x].length &&
        this.pixels[x][y + 1] === currentVal
      ) {
        this.paintFill(x, y + 1, newColor);
      }
      return;
    },

    handleColorSelect(i: number) {
      this.currentColor = this.colorsArray[i];
      // add border to box to show it is selected
    },

    isSelectedColor(i: number) {
      return this.currentColor === this.colorsArray[i];
    },

    resetGrid() {
      this.pixels = [
        [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0],
        [1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 1, 1],
        [1, 2, 2, 2, 2, 0, 1, 0, 1, 2, 2, 2, 2, 0, 1, 0],
        [1, 1, 1, 2, 2, 0, 1, 0, 1, 1, 1, 2, 2, 0, 1, 0],
        [1, 1, 1, 2, 2, 2, 2, 0, 1, 1, 1, 2, 2, 2, 2, 0],
        [1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1],
        [1, 1, 1, 1, 1, 2, 2, 1, 1, 1, 1, 1, 1, 2, 2, 1],
        [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
        [1, 0, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0],
        [1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0],
        [1, 2, 2, 2, 2, 0, 1, 0, 1, 2, 2, 2, 2, 0, 1, 0],
        [1, 1, 1, 2, 2, 0, 1, 0, 1, 1, 1, 2, 2, 0, 1, 0],
        [1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1],
        [1, 1, 1, 1, 1, 2, 2, 1, 1, 1, 1, 1, 1, 2, 2, 1],
        [1, 1, 1, 1, 1, 2, 2, 1, 1, 1, 1, 1, 1, 2, 2, 1],
      ];
    },

    handleMouseClick(e: MouseEvent) {
      const target = e.target as HTMLTextAreaElement;
      const [x, y] = target.id.split("-").map((i) => parseInt(i));
      if (!this.fill) {
        // change color of this box
        // this.backgroundColors[x - 1][y - 1] = this.currentColor;
        target.style.backgroundColor = this.currentColor;
      } else {
        // fill color
        // this.paintFill(x, y, this.currentColor);
      }
    },
  },
});
</script>

<style scoped>
.container {
  padding: 10px;
}

.color-picker-wrapper {
  display: flex;
  justify-content: center;
  padding: 10px;
}

.color-box {
  height: 30px;
  width: 30px;
  margin: 2px;
}

.box-border {
  border: 2px solid black;
}

.box {
  height: 30px;
  border-style: solid;
  border-width: 1px;
  border-color: #2c3e50;
}
.box0 {
  background-color: aquamarine;
}
.box1 {
  background-color: #f34213;
}
.box2 {
  background-color: whitesmoke;
}
.box3 {
  background-color: plum;
}
.button {
  padding: 6px 25px;
  font-size: 16px;
  background-color: white;
  color: black;
  border: 1px solid #2c3e50;
}

.button:hover {
  background-color: #2c3e50;
  color: white;
}

.rotate-center {
  -webkit-animation: rotate-center 0.6s ease-in-out both;
  animation: rotate-center 0.6s ease-in-out both;
}

/* ----------------------------------------------
 * Generated by Animista on 2021-5-20 8:55:46
 * Licensed under FreeBSD License.
 * See http://animista.net/license for more info. 
 * w: http://animista.net, t: @cssanimista
 * ---------------------------------------------- */

/**
 * ----------------------------------------
 * animation rotate-center
 * ----------------------------------------
 */
@-webkit-keyframes rotate-center {
  0% {
    -webkit-transform: rotate(0);
    transform: rotate(0);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@keyframes rotate-center {
  0% {
    -webkit-transform: rotate(0);
    transform: rotate(0);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
</style>
