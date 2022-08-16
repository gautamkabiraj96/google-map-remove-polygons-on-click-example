<template>
  <div id="app">
    <Polygons />
  </div>
</template>

<script>
import Polygons from "./components/Polygons";

export default {
  name: "App",
  components: {
    Polygons,
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.header-margins {
  margin-left: 40px;
  margin-top: 20px;
}

.map-margins {
  height: 400px;
  width: 600px;
  margin: 30px 40px;
}

.np-polygon-length {
  font-size: 14px;
  font-weight: bold;
}

.np-polygon-data {
  margin: 1em 0;
  font-size: 11px;
  background: #eee;
  padding: 4px;
}
</style>
