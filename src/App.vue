<template>

  <Grid msg="Grid Paint" />
  <footer>A <a href="http://celandau.com" target="_blank">Courtney Landau</a> project. </footer>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Grid from "./components/Grid.vue";

export default defineComponent({
  name: "App",
  components: {
    Grid,
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

footer {
  display: flex;
  padding: 5px;
  position: fixed;
  left: 0;
  bottom: 0;
}
a {
  color: #2c3e50;
}
</style>
