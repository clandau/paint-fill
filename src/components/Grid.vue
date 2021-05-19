<template>
  <div class="container">
    <div class="grid" :style="gridStyle">
      <div v-for="(pixelCol, i) in pixels" :key="pixelCol">
        <div
          v-for="(pixel, j) in pixelCol"
          :key="pixel"
          :class="'box box' + pixel"
          :id="`${i}-${j}`"
          v-on:click="handleColorChange"
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
        [1, 1, 1, 1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1, 1, 0, 0],
        [1, 0, 0, 1, 1, 0, 1, 1],
        [1, 2, 2, 2, 2, 0, 1, 0],
        [1, 1, 1, 2, 2, 0, 1, 0],
        [1, 1, 1, 2, 2, 2, 2, 0],
        [1, 1, 1, 1, 1, 2, 1, 1],
        [1, 1, 1, 1, 1, 2, 2, 1],
      ],
      pixelSize: "30px",
    };
  },

  computed: {
    gridStyle(): object {
      const columns = this.pixels[0].length;
      return {
        display: "grid",
        "justify-content": "center",
        "align-content": "center",
        "grid-template-columns": `repeat(${columns}, ${this.pixelSize})`,
      };
    },
  },

  methods: {
    handleColorChange(e: MouseEvent) {
      const target = e.target as HTMLTextAreaElement;
      console.log(target.id);
      const [x, y] = target.id.split("-").map(i => parseInt(i));
      console.log(x,y)
      // copy of pixels array
      // const pixelsCopy: number[][] = this.pixels.map(a => {
      //   return a.slice();
      // })
      // console.log(pixelsCopy)
      // this.pixels = this.paintFill(pixelsCopy, x, y, 3);
      this.paintFill(x, y, 3);
    },

    // paintFill(grid: number[][], x: number, y: number, newColor: number) {
    //   const currentVal = grid[x][y];
    //   // set currentVal to newColor
    //   grid[x][y] = newColor;

    //   // check top, bottom, left and right
    //   // if they match currentVal, call function with that val's coordinates
    //   // top
    //   if (x - 1 >= 0 && grid[x - 1][y] === currentVal) {
    //     this.paintFill(grid, x - 1, y, newColor);
    //   }
    //   // bottom
    //   if (x + 1 < grid.length && grid[x + 1][y] === currentVal) {
    //     this.paintFill(grid, x + 1, y, newColor);
    //   }
    //   // left
    //   if (y - 1 >= 0 && grid[x][y - 1] === currentVal) {
    //     this.paintFill(grid, x, y - 1, newColor);
    //   }
    //   // right
    //   if (
    //     y + 1 < grid[x].length &&
    //     grid[x][y + 1] === currentVal
    //   ) {
    //     this.paintFill(grid, x, y + 1, newColor);
    //   }
    //   return grid;
    // },
    paintFill(x: number, y: number, newColor: number) {
      const currentVal = this.pixels[x][y];
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
  },
});

    // function paintFill(grid: number[][], x: number, y: number, newColor: number) {
    //   console.log(x, y)
    //   const currentVal = grid[x][y];
    //   // set currentVal to newColor
    //   grid[x][y] = newColor;
    //   console.log(grid);
    //   // check top, bottom, left and right
    //   // if they match currentVal, call function with that val's coordinates
    //   // top
    //   if (x - 1 >= 0 && grid[x - 1][y] === currentVal) {
    //     paintFill(grid, x - 1, y, newColor);
    //   }
    //   // bottom
    //   if (x + 1 < grid.length && grid[x + 1][y] === currentVal) {
    //     paintFill(grid, x + 1, y, newColor);
    //   }
    //   // left
    //   if (y - 1 >= 0 && grid[x][y - 1] === currentVal) {
    //     paintFill(grid, x, y - 1, newColor);
    //   }
    //   // right
    //   if (
    //     y + 1 < grid[x].length &&
    //     grid[x][y + 1] === currentVal
    //   ) {
    //     paintFill(grid, x, y + 1, newColor);
    //   }
    //   return grid;
    // }

</script>

<style scoped>
.box {
  height: 30px;
  border-style: solid;
  border-width: 1px;
  border-color: navy;
}
.box0 {
  background-color: aquamarine;
}
.box1 {
  background-color: orangered;
}
.box2 {
  background-color: whitesmoke;
}
.box3 {
  background-color: plum;
}
</style>
