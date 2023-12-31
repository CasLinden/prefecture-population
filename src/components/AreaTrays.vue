<template>
  <div id="areas-container">
    <!-- split areas in half for desktop view -->
    <div
      v-for="halfAreas in [firstHalfAreas, secondHalfAreas]"
      :key="halfAreas[0].name"
      class="half-of-areas"
    >
      <!-- title, icon and CheckboxTray for each area -->
      <div
        v-for="(area, index) in halfAreas"
        :key="area.name"
        class="area-wrapper"
      >
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
          :startIndex="calculateStartIndex(index, halfAreas)"
          @prefecture-toggled="updateSelectedPrefCodes"
        />
      </div>
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
      // Hokkaido tray is open by default, and checked in CheckboxTray
      showingTrays: { 北海道: true },
      trayHeights: {},
      menuUpIcon:
        '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path fill="#" d="M7,15L12,10L17,15H7Z" /></svg>',
    };
  },
  computed: {
    firstHalfAreas() {
      return this.areas.slice(0, 4);
    },
    secondHalfAreas() {
      return this.areas.slice(4);
    },
  },
  methods: {
    toggleTray(areaName) {
      // if showingTrays[areaName] doesn't exist, it's created here
      this.showingTrays[areaName] = !this.showingTrays[areaName];
    },
    calculateStartIndex(index, halfAreas) {
      const isFirstHalf = halfAreas === this.firstHalfAreas;
      // calculate offset, 0 for first half, 23 for second
      const offset = isFirstHalf ? 0 : 23;
      // calculate the starting index based on count in areas
      return halfAreas
        .slice(0, index)
        .reduce((acc, cur) => acc + cur.count, offset);
    },
    updateSelectedPrefCodes(prefCode, isChecked) {
      // isChecked for adding, else for removing
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
  /* display: grid; */
  display: flex;
  flex-direction: column;
  padding: 0.4rem;
  gap: 5px;
}

.half-of-areas {
  display: flex;
  flex-direction: column;
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
  cursor: pointer;
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
/* no sticky hover */
@media (hover: hover) {
  .menu-icon.rotatedd:hover {
    transform: rotate(180deg);
  }
  .area:hover .menu-icon {
    transform: rotate(180deg);
  }
}
/* two columns for AreaTrays on desktop */
@media screen and (min-width: 769px) {
  #areas-container {
    flex-wrap: wrap;
    flex-direction: row;
    justify-content: space-around;
  }

  #areas-container > * {
    flex-basis: calc(50% - 5px);
    min-width: 0;
  }

  .area-wrapper {
    max-width: 50vw;
  }
}
</style>
