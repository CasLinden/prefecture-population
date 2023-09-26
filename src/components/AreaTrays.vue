<template>
  <div id="areas-container">
    <div v-for="(area, index) in areas" :key="area.name" class="area-wrapper">
      <div :id="area.name" class="area" @click="toggleTray(area.name)">
        <h2>{{ area.name }}</h2>
      </div>

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
      trayHeights: {},
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

<style scoped>
h2 {
  margin: 0;
  position: relative;
  z-index: 3;
  font-size: 1.6rem;
}
#areas-container {
  display: grid;
  gap: 5px;
}
.area {
  padding: 0.3rem 0rem 0.3rem 0rem;
  z-index: 2;
  height: 2.2rem;
  border: 1px solid black;
  border-radius: 15px;
  position: relative;
  background-color: white;
}

/* hiding layout under the area wrapper */
.area-wrapper {
  position: relative;
  transition: height 0.5s ease-in;
}

.area-wrapper::before {
  content: "";
  position: absolute;
  border-radius: 15px;
  /* height: calc(100% + 5px); */
  max-height: calc(2.2rem + 5px); /* 2.2 = h2 + area padding */
  top: -5px; /* grid gap on areas-container*/
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 1;
  background-color: white;
}
</style>
