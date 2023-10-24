<template>
  <div>
    <canvas ref="chart"></canvas>
  </div>
</template>

<script>
import { Doughnut } from 'vue-chartjs';

export default {
  extends: Doughnut,
  props: {
    data: {
      type: Array,
      required: true,
    },
  },
  mounted() {
    this.renderDonutChart();
  },
  methods: {
    renderDonutChart() {
      const chartData = {
        labels: this.data.map(item => item.destinationUserName),
        datasets: [
          {
            data: this.data.map(item => item['sum(cnt)']),
            backgroundColor: [
              'red', 'blue', 'green', 'orange', 'purple', 'pink', 'gray',
            ],
          },
        ],
      };

      this.renderChart(chartData, {
        responsive: true,
        maintainAspectRatio: false,
      });
    },
  },
};
</script>

<style scoped>

</style>
