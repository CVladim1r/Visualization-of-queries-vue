<template>
  <div>
    <apexchart :options="chartOptions" :series="chartSeries" type="bar" height="400" />
  </div>
</template>

<script>
import VueApexCharts from "vue3-apexcharts";
import data from "../assets/data.json";

export default {
  components: {
    apexchart: VueApexCharts,
  },
  data() {
    // Оставляем только уникальные значения destinationAddress
    const uniqueAddresses = [...new Set(data.map((item) => item.destinationAddress))];

    // Создаем объект, где ключи - уникальные адреса, значения - суммы для каждого адреса
    const groupedData = uniqueAddresses.reduce((result, address) => {
      const sum = data
        .filter((item) => item.destinationAddress === address)
        .reduce((acc, item) => acc + item["sum(cnt)"], 0);
      result[address] = sum;
      return result;
    }, {});

    // Создаем массив данных для графика
    const chartData = Object.entries(groupedData).map(([address, sum]) => ({
      x: sum < 50 ? "Другие" : address,
      y: sum,
    }));

    // Собираем адреса, сумма которых менее 50, в один столбец "Другие"
    const groupedChartData = chartData.reduce((result, item) => {
      if (item.x === "Другие") {
        if (result.length === 0) {
          result.push(item);
        } else {
          const lastIndex = result.length - 1;
          result[lastIndex].y += item.y;
        }
      } else {
        result.push(item);
      }
      return result;
    }, []);

    return {
      chartOptions: {
        chart: {
          id: "bar-chart",
        },
        xaxis: {
          categories: groupedChartData.map((item) => item.x),
          type: "category", // Тип оси X - категориальный
        },
        plotOptions: {
          bar: {
            columnWidth: "60%", // Устанавливаем ширину столбцов (в данном случае 60% от ширины доступной области)
          },
        },
        dataLabels: {
          enabled: false,
        },
        yaxis: {
          title: {
            text: "Сумма",
          },
        },
        responsive: [
          {
            breakpoint: 480,
            options: {
              chart: {
                width: "100%", // Ширина графика на мобильных устройствах
                height: 300, // Высота графика на мобильных устройствах
              },
              plotOptions: {
                bar: {
                  columnWidth: "100%", // Установка ширины столбцов на мобильных устройствах
                },
              },
              xaxis: {
                type: "category", // Тип оси X - категориальный
              },
            },
          },
        ],
        fill: {
          opacity: 1,
        },
        tooltip: {
          enabled: false,
        },
      },
      chartSeries: [
        {
          name: "Кол-во sum(cnt)",
          data: groupedChartData.map((item) => item.y),
        },
      ],
    };
  },
};
</script>
