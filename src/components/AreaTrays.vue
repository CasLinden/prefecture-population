<template>
  <div id="areas-container">
    <div v-for="(area, index) in areas" :key="area.name" class="area-wrapper">
      <div :id="area.name" class="area" @click="toggleTray(area.name)">
        <h2>{{ area.name }}</h2>
        <div
          class="menu-icon"
          :class="{ rotated: showingTrays[area.name] }"
          v-html="menuUpIcon"
        ></div>
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
      selectedPrefCodes: [1],
      showingTrays: { 北海道: true },
      trayHeights: {},
      menuUpIcon:
        '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path fill="#" d="M7,15L12,10L17,15H7Z" /></svg>',
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
  font-size: 1.5rem;
}
#areas-container {
  display: grid;
  padding: 0.4rem;
  gap: 5px;
}
.area {
  padding: 0.3rem 0rem 0.3rem 0rem;
  z-index: 2;
  height: 3rem;
  border: 1px solid #2c3e50;
  border-radius: 15px;
  position: relative;
  background-color: white;
  display: flex;
  justify-content: center;
  align-items: center;
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
  max-height: calc(3.6rem + 5px); /* 2.2 = h2 + area padding */
  top: -5px; /* grid gap on areas-container*/
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 1;
  background-color: var(--background-grey);
}

.menu-icon {
  width: 3rem;
  height: 3rem;
  transition: transform 0.2s ease-in-out;
}

.menu-icon svg {
  width: 100%;
  height: 100%;
}

.menu-icon.rotated {
  transform: rotate(180deg);
}

@media (hover: hover) {
  .menu-icon.rotatedd:hover {
    transform: rotate(180deg);
  }
  .area:hover .menu-icon {
    transform: rotate(180deg);
  }
}
</style>
