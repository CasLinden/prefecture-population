<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="dataForChart">
      <PopulationChart :chartData="dataForChart"></PopulationChart>
    </div>
    <div>
      <AreaTrays @prefecture-toggled="handlePrefectureToggled"></AreaTrays>
    </div>
  </div>
</template>

<script>
import prefectures from "@/prefectures.json";
import PopulationChart from "./PopulationChart.vue";
import AreaTrays from "./AreaTrays.vue";

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
        // immidately prep and update the chart data:
        const preparedData = this.prepareDataForChart(data);
        this.updateChartData(preparedData);
      } catch (error) {
        console.error("There was a problem with the fetch operation:", error);
      } finally {
        this.loading = false;
      }
    },
    prepareDataForChart(data) {
      const populationTypes = [
        "総人口",
        "年少人口",
        "生産年齢人口",
        "老年人口",
      ];
      const years = data.result.data[0].data
        .map((item) => item.year.toString())
        .slice(0, 14);

      const datasets = populationTypes.map((type) => {
        const populationTypeData = data.result.data.find(
          (item) => item.label === type
        );
        const population = populationTypeData.data
          .map((item) => item.value)
          .slice(0, 14);

        return {
          label: `${prefectures[this.selectedPrefCode - 1].name}`,
          data: population,
          borderWidth: 2,
          fill: false,
          prefCode: `${this.selectedPrefCode}`,
          populationDataType: type,
        };
      });

      return {
        labels: years,
        datasets: datasets,
      };
    },
    updateChartData(preparedData) {
      if (!this.dataForChart) {
        this.dataForChart = preparedData;
      } else {
        this.dataForChart.datasets.push(...preparedData.datasets);
      }
    },
  },
  components: {
    PopulationChart,
    AreaTrays,
  },
};
</script>
