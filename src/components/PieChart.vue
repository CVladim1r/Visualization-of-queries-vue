<template>
    <div>
      <h2>Pie Chart</h2>
      <Doughnut :data="chartData" :options="chartOptions" />
    </div>
  </template>
  
  <script>
  import { Doughnut } from 'vue-chartjs';
  import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js';
  import axios from 'axios';
  
  ChartJS.register(ArcElement, Tooltip, Legend);
  
  export default {
    name: 'PieChart',
    extends: Doughnut,
    data() {
      return {
        data: [], // To store the data fetched from d.json
      };
    },
    computed: {
      chartData() {
        const labels = this.data.map((item) => item.destinationAddress);
        const data = this.data.map((item) => item['sum(cnt)']);
  
        return {
          labels,
          datasets: [
            {
              backgroundColor: ['#41B883', '#E46651', '#00D8FF', '#DD1B16'],
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
      axios
        .get('/d.json') // Assuming d.json is located in the public directory
        .then((response) => {
          this.data = response.data;
          this.renderChart(this.chartData, this.chartOptions);
        })
        .catch((error) => {
          console.error('Error fetching data:', error);
        });
    },
  };
  </script>
  