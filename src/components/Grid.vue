<template>
  <div class="main-container" @mouseup="isDrawing = false">
    <div class="container">
      <h1>{{ msg }}</h1>
      <p>Paint the grid!</p>
      <div class="color-picker-wrapper">
        <div
          v-for="(color, i) of colorsArray"
          :class="['color-box', { 'box-border': isSelectedColor(i) }]"
          :style="`background-color: ${color};`"
          @click="handleColorSelect(i)"
          :key="i"
        ></div>
      </div>
      <div>
        <div class="switch">
          <input
            type="checkbox"
            class="switch-input"
            id="fill-switch"
            v-model="fill"
          />
          <label for="fill-switch" class="switch-label">Fill</label>
        </div>
        <span class="container">Fill</span>
      </div>
    </div>
    <div class="grid" :style="gridStyle" :class="{ 'rotate-center': isOneColor }">
      <div v-for="(col, i) of cells" :key="i">
        <div
          v-for="(row, j) of col"
          :key="i - j"
          class="box"
          :style="{ backgroundColor: row.color, height: pixelSize }"
          @mousedown="handleMouseDown(i, j)"
          @mousemove="handleMouseMove(i, j)"
          @click="handleMouseClick(i, j)"
        ></div>
      </div>
    </div>
    <div class="container">
      <button class="button" @click="resetGrid">Reset Grid</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { Cell } from "../types";

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
      numCols: 30,
      numRows: 30,
      pixelSize: "25px",
      initialColor: "white",
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
        "#565656",
        "#B2FFA9",
        "#FF4A1C",
        "#81523F",
        "#3F2A2B",
      ],
      cells: [] as Cell[][],
      currentColor: "",
      fill: false,
      isDrawing: false,
    };
  },
  created() {
    let i = 0;
    let j = 0;
    while (i < this.numCols) {
      this.cells.push([]);
      while (j < this.numRows) {
        this.cells[i].push({ color: this.initialColor });
        j++;
      }
      i++;
      j = 0;
    }
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
      // TODO add check here
      const flatCells: Cell[] = this.cells.flat();
      return flatCells.every(item => {
        return item.color === flatCells[0].color && item.color !== "white"
      })
    },
  },

  methods: {
    handleColorSelect(i: number) {
      // add border to box to show it is selected
      this.currentColor = this.colorsArray[i];
    },

    handleMouseDown(y: number, x: number) {
      if (!this.fill) {
        this.isDrawing = true;
        this.cells[y][x].color = this.currentColor;
      }
    },

    handleMouseMove(y: number, x: number) {
      if (this.isDrawing) {
        this.cells[y][x].color = this.currentColor;
      }
    },

    handleMouseClick(y: number, x: number) {
      if (this.fill) {
        this.paintFill(y, x);
      }
    },

    handleMouseUp() {
      this.isDrawing = false;
    },
    paintFill(x: number, y: number) {
      const currentVal = this.cells[x][y].color;
      if (currentVal === this.currentColor) return;
      // set currentVal to newColor
      this.cells[x][y].color = this.currentColor;
      // check top, bottom, left and right
      // if they match currentVal, call function with that val's coordinates
      // top
      if (x - 1 >= 0 && this.cells[x - 1][y].color === currentVal) {
        this.paintFill(x - 1, y);
      }
      // bottom
      if (
        x + 1 < this.cells.length &&
        this.cells[x + 1][y].color === currentVal
      ) {
        this.paintFill(x + 1, y);
      }
      // left
      if (y - 1 >= 0 && this.cells[x][y - 1].color === currentVal) {
        this.paintFill(x, y - 1);
      }
      // right
      if (
        y + 1 < this.cells[x].length &&
        this.cells[x][y + 1].color === currentVal
      ) {
        this.paintFill(x, y + 1);
      }
      return;
    },

    isSelectedColor(i: number) {
      return this.currentColor === this.colorsArray[i];
    },

    resetGrid() {
      this.cells.map((col) => {
        col.map((row) => {
          row.color = this.initialColor;
        });
      });
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
  border-style: solid;
  border-width: 1px;
  border-color: #909aa3;
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
  border: 1px solid #97afc7;
}

.button:hover {
  background-color: #2c3e50;
  color: white;
}

.switch {
  position: relative;
  display: inline-block;
}

.switch-input {
  display: none;
}
.switch-label {
  display: block;
  width: 48px;
  height: 24px;
  text-indent: -150%;
  clip: rect(0 0 0 0);
  color: transparent;
  user-select: none;
  /* position: relative;
    padding-left: 2.3em;
    font-size: 1.05em;
    line-height: 1.7;
    cursor: pointer; */
}
.switch-label::before,
.switch-label::after {
  content: "";
  display: block;
  position: absolute;
  cursor: pointer;
}
.switch-label::before {
  width: 100%;
  height: 100%;
  background-color: #dedede;
  border-radius: 9999em;
  -webkit-transition: background-color 0.25s ease;
  transition: background-color 0.25s ease;
}
.switch-label::after {
  top: 0;
  left: 0;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background-color: #fff;
  box-shadow: 0 0 2px rgba(0, 0, 0, 0.45);
  -webkit-transition: left 0.25s ease;
  transition: left 0.25s ease;
}
.switch-input:checked + .switch-label::before {
  background-color: #277da1;
}
.switch-input:checked + .switch-label::after {
  left: 24px;
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
