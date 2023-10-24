<template>
    <div>
      <h2>Bar Chart with Gaps</h2>
      <Bar :data="chartData" :options="chartOptions" />
    </div>
  </template>
  
  <script>
  import { Bar } from 'vue-chartjs';
  import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';
  
  ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);
  
  export default {
    name: 'BarChart',
    extends: Bar,
    props: ['data'],
    computed: {
      chartData() {
        const labels = this.data.map((item) => item.destinationAddress);
        const data = this.data.map((item) => item['sum(cnt)']);
  
        return {
          labels,
          datasets: [
            {
              label: 'Destination Address',
              backgroundColor: '#3399FF', // Color of the bars
              data,
            },
          ],
        };
      },
      chartOptions() {
        return {
          responsive: true,
          maintainAspectRatio: false,
        };
      },
    },
    mounted() {
      this.renderChart(this.chartData, this.chartOptions);
    },
  };
  </script>
  