<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="dataForChart">
      <PopulationChart :chartData="dataForChart"></PopulationChart>
    </div>
    <div>
      <PrefectureCheckboxes @prefecture-toggled="handlePrefectureToggled">
      </PrefectureCheckboxes>
    </div>
  </div>
</template>

<script>
import prefectures from "@/prefectures.json";
import PopulationChart from "./PopulationChart.vue";
import PrefectureCheckboxes from "./PrefectureCheckboxes.vue";

export default {
  data() {
    return {
      dataForChart: null,
      selectedPrefCode: null,
      loading: false,
      prefectures,
    };
  },
  methods: {
    handlePrefectureToggled({ prefCode, isChecked }) {
      if (isChecked) {
        this.selectedPrefCode = prefCode;
        this.fetchPopulationData();
      } else {
        this.dataForChart = {
          ...this.dataForChart,
          datasets: this.dataForChart.datasets.filter(
            (dataset) => +dataset.prefCode !== prefCode
          ),
        };
      }
    },
    async fetchPopulationData() {
      this.loading = true;
      const apiKey = process.env.VUE_APP_RESAS_KEY;
      const url = `https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear?prefCode=${this.selectedPrefCode}&cityCode=-`;
      try {
        const response = await fetch(url, {
          headers: {
            "X-API-KEY": apiKey,
          },
        });

        if (!response.ok) {
          throw new Error("Network response was not ok");
        }

        const data = await response.json();
        const totalPopulationData = data.result.data.find(
          (item) => item.label === "総人口"
        );

        const years = totalPopulationData.data
          .map((item) => item.year.toString())
          .slice(0, 14);
        const population = totalPopulationData.data
          .map((item) => item.value)
          .slice(0, 14);

        if (!this.dataForChart) {
          this.dataForChart = {
            labels: years,
            datasets: [
              {
                label: `総人口`,
                data: population,
                borderColor: "rgba(75, 192, 192, 1)",
                borderWidth: 2,
                fill: false,
                prefCode: `${this.selectedPrefCode}`,
              },
            ],
          };
        } else {
          this.dataForChart.datasets.push({
            label: `総人口`,
            data: population,
            borderColor: "rgba(75, 192, 192, 1)",
            borderWidth: 2,
            fill: false,
            prefCode: `${this.selectedPrefCode}`,
          });
        }
      } catch (error) {
        console.error("There was a problem with the fetch operation:", error);
      } finally {
        this.loading = false;
        console.log(this.dataForChart);
      }
    },
  },
  components: {
    PopulationChart,
    PrefectureCheckboxes,
  },
};
</script>
