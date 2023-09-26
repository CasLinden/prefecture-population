<template>
  <PopulationTypeToggle @type-changed="onTypeChanged"></PopulationTypeToggle>
  <div id="chart-container">
    <Line
      id="chart-canvas"
      :data="filteredChartData || null"
      :options="options"
    />
  </div>
</template>

<script>
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  PointElement,
  CategoryScale,
  LinearScale,
  LineElement,
  Colors,
} from "chart.js";
import { Line } from "vue-chartjs";
import PopulationTypeToggle from "./PopulationTypeToggle.vue";

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  Title,
  Tooltip,
  Legend,
  LineElement,
  Colors
);

export default {
  name: "PopulationChart",
  props: {
    chartData: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      currentType: "総人口",
      options: {
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
  computed: {
    filteredChartData() {
      if (!this.currentType || !this.chartData.datasets) {
        console.log("returned because there is no currentType or chartData");
        return this.chartData;
      }
      console.log(this.chartData.datasets);
      const filteredDatasets = this.chartData.datasets.filter(
        (dataset) => dataset.populationDataType === this.currentType
      );

      return {
        ...this.chartData,
        datasets: filteredDatasets,
      };
    },
  },
  components: {
    Line,
    PopulationTypeToggle,
  },
  methods: {
    onTypeChanged(type) {
      this.currentType = type;
    },
  },
};
</script>

<style scoped>
#chart-container {
  margin-bottom: 1rem;
}

#chart-container {
  height: 40vh;
}
/* @media (max-width: 700px) { */
/* } */
</style>
