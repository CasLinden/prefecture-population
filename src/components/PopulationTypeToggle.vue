<template>
  <div id="type-toggle-panel">
    <div
      v-for="type in populationTypes"
      :key="type"
      class="population-type-toggle"
      :class="{ 'current-population-type': currentType === type }"
      @click="togglePopulationType(type)"
    >
      {{ type }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentType: "総人口",
      populationTypes: ["総人口", "年少人口", "生産年齢人口", "老年人口"],
    };
  },
  methods: {
    // inform populationChart to filter Data for population data type
    togglePopulationType(type) {
      if (this.currentType === type) return;
      this.currentType = type;
      this.$emit("type-changed", this.currentType);
    },
  },
};
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
  border-radius: 15px;
}

.current-population-type {
  background-color: #ff6384;
  color: white;
}
</style>
