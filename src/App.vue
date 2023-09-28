<template>
  <div id="app-container">
    <SiteHeader></SiteHeader>
    <PopulationFetcher></PopulationFetcher>
  </div>
</template>

<script>
import SiteHeader from "./components/SiteHeader.vue";
import PopulationFetcher from "./components/PopulationFetcher.vue";

export default {
  name: "App",
  components: {
    SiteHeader,
    PopulationFetcher,
  },
};
</script>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

:root {
  --main-text-color: #2c3e50;
  --background-accent: #e9ebed;
}

body {
  background-color: var(--background-accent);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  box-sizing: border-box;
  color: var(--main-text-color);
}
</style>
