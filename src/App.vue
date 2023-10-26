<template>
  <section class="main-hub">
    <h1>Vue App</h1>
    <div class="radar-and-pie">
      <div>
        <RadarView :targets="radarData" />
      </div>
      <div>
        <PieChart />
      </div>
    </div>
    
    <div>
      <bar-chart />
    </div>
    <div>
      <h1>Active Channels Table</h1>
      <TableActiveChannel />
    </div>
  </section>
</template>

<script>

// Наши компоненты
import RadarView from './components/RadarView.vue';
import TableActiveChannel from './components/TableActiveChannel.vue'; 
import PieChart from "./components/PieChart.vue";
import BarChart from "./components/BarChart.vue";


export default {
  name: "App",

  components: {
    TableActiveChannel,
    RadarView,
    PieChart,
    BarChart,
  },





// Просто засоряем фронт
// Временное решение, надо доработать server.py
  data() {
    return {

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
      
    };
  },

 
};
</script>



<style scoped>

h1{
  color: aliceblue;
}.radar {
  position: relative;
  width: 400px;
  height: 400px;
  border: 2px solid #000;
  background-color: #252527
}
.main-hub{
  background-color: #252527
}
#app {
  background-color: #252527}
.target {
  position: relative;
}

.radar-and-pie{
  display: flex;
}
</style>
