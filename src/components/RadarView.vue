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
      data: {
        labels,
        datasets: [{
          label: 'Targets',
          data,
          backgroundColor: 'rgba(255, 255, 255, 0.2)', // изменен на белый
          borderColor: 'rgba(255,255,255,1)', // изменен на белый
          pointBackgroundColor: 'rgba(255,255,255,1)', // изменен на белый
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
              color: '#888' // добавлены серые линии углов
            },
            grid: {
              color: '#555' // добавлена темная сетка
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
