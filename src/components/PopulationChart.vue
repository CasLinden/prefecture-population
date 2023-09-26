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
        return this.chartData;
      }
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
  position: relative;
  padding-right: 1rem;
  padding-left: 1rem;
}
</style>
