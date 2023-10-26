<template>
  <div>
    <canvas ref="chart"></canvas>
  </div>
</template>

<script>
import { Chart, RadarController, LinearScale, PointElement, LineElement, Title, Tooltip, Legend, RadialLinearScale } from 'chart.js';
import 'chartjs-adapter-date-fns';

Chart.register(RadarController, LinearScale, PointElement, LineElement, Title, Tooltip, Legend, RadialLinearScale);

export default {
  props: {
    targets: Array,
  },
  
  mounted() {
    const labels = this.targets.map(target => target.number);
    const data = this.targets.map(target => target.cnt);
    const ctx = this.$refs.chart.getContext('2d');
    
    new Chart(ctx, {
      type: 'radar',

      // Временное решение, надо доработать server.py
      data: {
        labels,
        datasets: [{
          label: 'Цели',
          data,
          backgroundColor: 'rgba(255, 255, 255, 0.2)',
          borderColor: '#579cd7',
          pointBackgroundColor: 'rgba(255,255,255,1)', 
          pointBorderColor: '#fff',
          pointHoverBackgroundColor: '#fff',
          pointHoverBorderColor: 'rgba(255,99,132,1)',
        }],
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
          r: {
            angleLines: {
              color: '#888'  // Добавлены серые линии углов
            },
            grid: {
              color: '#555' // Добавлена темная сетка
            }
          }
        }
      },
    });
  },
};
</script>

<style scoped>
canvas {
  width: 100%;
  height: 400px;
}
</style>
