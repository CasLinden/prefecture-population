<template>
  <div id="chart-container">
    <Line id="chart-canvas" :data="chartData" :options="options" />
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
      options: {
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
  components: {
    Line,
  },
};
</script>

<style scoped>
/* @media (max-width: 700px) { */
#chart-container {
  height: 40vh;
}
/* } */
</style>
