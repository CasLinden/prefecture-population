<template>
  <div>
    <div id="display">
      <div id="loading" v-if="loading">読み込み中...</div>
      <div id="display-wrapper" v-else-if="dataForChart">
        <PopulationChart :chartData="dataForChart"></PopulationChart>
      </div>
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
      // dataForChart will be a Proxy Object, modified by handlePrefectureToggled() and fetchPopulationData()
      dataForChart: null,
      // 1 = Hokkaido (we show the Hokkaido graph by default)
      selectedPrefCode: 1,
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
        // we "reset" dataForChart completely, using the spread operator
        // if we only update datasets, vue does not rerender the chart
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
        // prep and update the chart data:
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
        // separate the data per type
        const populationTypeData = data.result.data.find(
          (item) => item.label === type
        );
        // prep the data we want for each type
        const population = populationTypeData.data
          .map((item) => item.value)
          .slice(0, 14);
        // return four different datasets
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
        // vue seems to notice that dataForChart changed on .push, so we don't need to "reset" the dataForChart object
        this.dataForChart.datasets.push(...preparedData.datasets);
      }
    },
  },
  mounted() {
    this.fetchPopulationData();
  },
  components: {
    PopulationChart,
    AreaTrays,
  },
};
</script>

<style scoped>
#display {
  min-height: 50vh;
  display: grid;
  grid-template-rows: 1fr;
  grid-template-columns: 1fr;
  border-top: 0.1px grey solid;
  border-bottom: 0.1px grey solid;
  padding-top: 1rem;
  padding-bottom: 5rem;
  margin-bottom: 1rem;
  background-color: white;
}

@media screen and (max-width: 768px) {
  #display {
    padding-bottom: 2rem;
    padding-top: 0rem;
  }
}

#display-wrapper {
  width: 100%;
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: auto 1fr;
}

#loading {
  font-size: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 40vh;
}
</style>
