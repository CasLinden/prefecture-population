<template>
  <div id="checkboxes-container">
    <div v-for="area in areas" :key="area.name" :id="area.name" class="地域">
      <h3>{{ area.name }}</h3>
      <div
        v-for="pref in getPrefecturesForArea(area)"
        :key="pref.code"
        :class="pref"
      >
        <input
          type="checkbox"
          :value="pref.code"
          :id="pref.code"
          @change="updateSelectedPrefCodes(pref.code, $event.target.checked)"
        />
        <label :for="pref.code">{{ pref.name }}</label>
      </div>
    </div>
  </div>
</template>

<script>
import prefectures from "@/prefectures.json";
import chiiki from "@/chiiki.json";

export default {
  data() {
    return {
      areas: chiiki,
      prefectures: prefectures,
    };
  },
  methods: {
    getPrefecturesForArea(area) {
      const startIndex = this.areas
        .slice(0, this.areas.indexOf(area))
        .reduce((acc, curr) => acc + curr.count, 0);

      return this.prefectures.slice(startIndex, startIndex + area.count);
    },
    updateSelectedPrefCodes(prefCode, isChecked) {
      this.$emit("prefecture-toggled", { prefCode, isChecked });
    },
  },
};
</script>

<style scoped>
#checkboxes-container {
  display: flex;
  justify-content: space-between;
  padding: 1rem;
  flex-wrap: wrap;
}
.地域 {
  display: flex;
  flex-direction: column;
  align-items: flex-startrt;
  flex-wrap: wrap;
}
.地域 h3 {
  align-self: center;
}
</style>
