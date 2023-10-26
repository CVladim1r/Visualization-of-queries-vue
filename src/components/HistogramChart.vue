<template>
  <div>
    <!-- Выбор типа графика с помощью кнопок -->
    <div>
      <button @click="changeChartType('bar')">Гистограмма</button>
      <button @click="changeChartType('pie')">Круговая диаграмма</button>
      <button @click="changeChartType('bar-with-gaps')">Гистограмма с разрывами</button>
      <button @click="changeChartType('table')">Табличное отображение</button>
    </div>

    <!-- Отображение графика в зависимости от выбранного типа -->
    <div>
      <template v-if="currentChartType === 'bar'">
        <vue-apex-charts :options="chartOptions" :series="chartSeries"></vue-apex-charts>
      </template>
      <template v-else-if="currentChartType === 'pie'">
        <vue-apex-charts type="pie" :options="pieChartOptions" :series="pieChartSeries"></vue-apex-charts>
      </template>
      <template v-else-if="currentChartType === 'bar-with-gaps'">
        <vue-apex-charts :options="barWithGapsOptions" :series="chartSeries"></vue-apex-charts>
      </template>
      <template v-else-if="currentChartType === 'table'">
        <!-- Вставьте здесь код для табличного отображения -->
      </template>
    </div>
  </div>
</template>

<script>
import VueApexCharts from 'vue3-apexcharts';
import data from '../assets/data.json';

export default {
  components: {
    VueApexCharts,
  },
  data() {
    const chartData = data.map(item => ({
      x: item.destinationAddress,
      y: item['sum(cnt)'],
    }));

    return {
      chartOptions: {
        chart: {
          height: 350,
          type: 'bar',
        },
        plotOptions: {
          bar: {
            horizontal: false,
          },
        },
        xaxis: {
          title: {
            text: 'Сумма (cnt)',
          },
        },
      },
      chartSeries: [
        {
          data: chartData,
        },
      ],
      pieChartOptions: {
        chart: {
          height: 350,
          type: 'pie',
        },
      },
      pieChartSeries: [
        {
          data: chartData,
        },
      ],
      barWithGapsOptions: {
        chart: {
          height: 350,
          type: 'bar',
        },
        plotOptions: {
          bar: {
            horizontal: false,
            columnWidth: '50%', // Установите ширину колонок по вашему выбору
          },
        },
        xaxis: {
          title: {
            text: 'Сумма (cnt)',
          },
        },
      },
      currentChartType: 'bar', // Начнем с гистограммы
    };
  },
  methods: {
    changeChartType(type) {
      this.currentChartType = type;
    },
  },
};
</script>
