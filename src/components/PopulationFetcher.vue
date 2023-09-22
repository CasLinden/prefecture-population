<template>
  <div>
    <label for="prefecture">Choose a Prefecture:</label>
    <select v-model="selectedPrefCode" @change="fetchPopulationData">
      <option v-for="pref in prefectures" :key="pref.code" :value="pref.code">
        {{ pref.name }}
      </option>
    </select>
    <div v-if="loading">Loading...</div>
    <div v-else>
      <p>Total Population: {{ totalPopulation }}</p>
    </div>
  </div>
</template>

<script>
import prefectures from "@/prefectures.json";

export default {
  data() {
    return {
      selectedPrefCode: null,
      totalPopulation: null,
      loading: false,
      prefectures,
    };
  },
  methods: {
    async fetchPopulationData() {
      this.loading = true;
      const apiKey = process.env.VUE_APP_RESAS_KEY;
      const url = `https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear?prefCode=${this.selectedPrefCode}&cityCode=-`;

      const response = await fetch(url, {
        headers: {
          "X-API-KEY": apiKey,
        },
      });

      const data = await response.json();
      const totalPopulationData = data.result.data.find(
        (item) => item.label === "総人口"
      );
      console.log(totalPopulationData);

      // Assuming you want the latest available data
      const latestData = totalPopulationData.data.slice(-1)[0];

      this.totalPopulation = latestData
        ? latestData.value
        : "Data not available";
      this.loading = false;
    },
  },
};
</script>
