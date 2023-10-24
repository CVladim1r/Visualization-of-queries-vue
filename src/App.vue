<template>
  <section class="main-hub">
    <div id="app">
      <h1>Active Channels Table</h1>
      <TableActiveChannel />
    </div>
    <div id="app">
      <RadarView :targets="radarData" />
    </div>
    <div>
     
    </div>
    <div id="app">
      <PieChart :data="jsonData" />
      <BarChart :data="jsonData" />

    </div>


    
  </section>
</template>

<script>
import RadarView from './components/RadarView.vue';
import TableActiveChannel from './components/TableActiveChannel.vue'; 
//import DonutChartComponent from './components/DonutChartComponent.vue';   <DonutChartComponent :data="donutChartData" /> 
import jsonData from './assets/diagram.json'; // JSON-файл

import PieChart from "./components/PieChart.vue";
import BarChart from "./components/BarChart.vue";


export default {
  name: "App",

  components: {
    TableActiveChannel,
    // DonutChartComponent,
    RadarView,

    PieChart,
    BarChart,
  },
  data() {
    return {
      jsonData: [

      ],
      radarData: [
      {
        "argument" : "Events",
        "number" : 22,
        "max(srt)" : 1695163267,
        "min(srt)" : 1695138073,
        "cnt" : 139367
      },
      {
        "argument" : "Events",
        "number" : 23,
        "max(srt)" : 1695188460,
        "min(srt)" : 1695163276,
        "cnt" : 140910
      },
      {
        "argument" : "Events",
        "number" : 6,
        "max(srt)" : 1694748373,
        "min(srt)" : 1694734888,
        "cnt" : 69843
      },
      {
        "argument" : "Events",
        "number" : 24,
        "max(srt)" : 1695196771,
        "min(srt)" : 1695188475,
        "cnt" : 45926
      },
      {
        "argument" : "Events",
        "number" : 5,
        "max(srt)" : 1694734863,
        "min(srt)" : 1694709679,
        "cnt" : 130543
      },
      {
        "argument" : "Events",
        "number" : 3,
        "max(srt)" : 1694684466,
        "min(srt)" : 1694671324,
        "cnt" : 70163
      },
      {
        "argument" : "Events",
        "number" : 21,
        "max(srt)" : 1695138069,
        "min(srt)" : 1695122231,
        "cnt" : 115273
      },
      {
        "argument" : "Events",
        "number" : 4,
        "max(srt)" : 1694709664,
        "min(srt)" : 1694684491,
        "cnt" : 132804
      }
      ],
      donutChartData: jsonData[
        "select destinationUserName, SUM(cnt) from (SELECT destinationUserName, COUNT(destinationUserName) as cnt FROM savrus.data where srtd >= '2023-09-12' AND srtd <= '2023-09-20' AND (srt >= 1694608869) AND (srt <= 1695213669) and notEmpty(toString(destinationUserName)) GROUP BY destinationUserName ) group by destinationUserName ORDER BY sum(cnt) DESC"
      ],

      
    };
  },
  created() {
    // Загрузка данных из JSON файла
    fetch("/d.json")
    .then((response) => response.json())
    .then((data) => {
      this.jsonData = data;
      console.log("Data loaded:", data);
    });
  },

 
};
</script>



<style scoped>
.radar {
  position: relative;
  width: 400px;
  height: 400px;
  border: 2px solid #000;
  background-color: #3d3d3d; 
}
#app {
  background-color: rgb(75, 75, 75)}
.target {
  position: relative;
}

</style>
