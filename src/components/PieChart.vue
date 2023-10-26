<template>
  <div>
    <apexchart class="donut" type="donut" :options="chartOptions" :series="series" style="width: 550px; height: 550px; color: #fff;"></apexchart>
  </div>
</template>

<script>
import VueApexCharts from 'vue3-apexcharts'
import data from '../assets/pie-chart.json'

export default {
  name: 'DonutChart',
  components: {
    apexchart: VueApexCharts,
  },
  data() {
    return {
      series: [],
      chartOptions: {
        chart: {
          width: 120, // Фиксированная ширина 100px
          height: 120, // Фиксированная высота 100px
          type: 'donut',
        },
        colors: ['#579cd7', '#636bf2', '#c17b40', '#fa486c'], //Цвета для круговой диаграммы
        labels: [],
        plotOptions: {
          pie: {
            size: 100, // Размер круга (100%)
            customScale: 0.8,
            donut: {
              size: '60%', // Размер дырки внутри (60%)
            },
          },
        },
        legend: {
          position: 'bottom',
          color: "#579cd7",
        },
      },
    }
  },
  mounted() {
    try {
      const labels = []
      const series = []
      // Временное решение, надо доработать server.py
      for (const { destinationUserName, 'sum(cnt)': sumCnt } of data) {
        labels.push(destinationUserName)
        series.push(sumCnt)
      }
      this.series = series
      this.chartOptions.labels = labels
      this.$refs.chart.updateOptions(this.chartOptions)
    } catch (error) {
      console.error('Ошибка при обработке данных:', error)
      // Дополнительные действия по обработке ошибки, если необходимо
    }
  },
}
</script>

<style >
</style>