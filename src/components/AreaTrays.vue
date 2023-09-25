<template>
  <div id="checkboxes-container">
    <div
      v-for="(area, index) in areas"
      :key="area.name"
      :id="area.name"
      class="area"
    >
      <h2 @click="toggleTray(area.name)">{{ area.name }}</h2>
      <CheckboxTray
        :showTray="showingTrays[area.name]"
        :area="area"
        :selectedPrefCodes="selectedPrefCodes"
        :allPrefectures="prefectures"
        :startIndex="calculateStartIndex(index)"
        @prefecture-toggled="updateSelectedPrefCodes"
      />
    </div>
  </div>
</template>

<script>
import areas from "@/areas.json";
import prefectures from "@/prefectures.json";
import CheckboxTray from "./CheckboxTray.vue";

export default {
  components: {
    CheckboxTray,
  },
  data() {
    return {
      areas: areas,
      prefectures: prefectures,
      selectedPrefCodes: [],
      showingTrays: {},
    };
  },
  methods: {
    toggleTray(areaName) {
      this.showingTrays[areaName] = !this.showingTrays[areaName];
    },
    calculateStartIndex(index) {
      return this.areas
        .slice(0, index)
        .reduce((acc, cur) => acc + cur.count, 0);
    },
    updateSelectedPrefCodes(prefCode, isChecked) {
      if (isChecked) {
        this.selectedPrefCodes.push(prefCode);
      } else {
        this.selectedPrefCodes = this.selectedPrefCodes.filter(
          (code) => code !== prefCode
        );
      }
      this.$emit("prefecture-toggled", { prefCode, isChecked });
    },
  },
};
</script>
