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
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 0.2rem;
  box-sizing: border-box;
}
</style>
