<template>
  <div id="type-toggle-panel">
    <div class="population-type-toggle current-population-type">総人口</div>
    <div class="population-type-toggle">年少人口</div>
    <div class="population-type-toggle">生産年齢人口</div>
    <div class="population-type-toggle">老年人口</div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
#type-toggle-panel {
  box-sizing: border-box;
  display: flex;
  justify-content: space-between;
  margin: 1rem 0 1rem 0;
}

.population-type-toggle {
  font-size: 0.8rem;
  padding: 0.2rem 1rem 0.2rem 1rem;
  white-space: nowrap;
  color: #aaaaaa;
}
</style>
