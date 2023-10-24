<template>
  <div class="table-container">
    <input v-model="searchText" placeholder="Поиск..." class="search-input" />
    <table class="custom-table">
      <thead>
        <tr>
          <th>SvrSeid</th>
          <th>SvrSrt</th>
          <th>Seid</th>
          <th>SRT</th>
          <th>Name</th>
          <th>DeviceAddress</th>
          <th>DeviceHostName</th>
          <th>Severity</th>
          <th>DestinationAddress</th>
          <th>DestinationUserName</th>
          <th>SourceAddress</th>
          <th>SourceUserName</th>
          <th>DeviceVendor</th>
          <th>DeviceProduct</th>
          <th>AgentAddress</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in tableData" :key="item.SvrSeid" :class="{ 'highlighted-row': isItemHighlighted(item) }">
          <td>{{ item.SvrSeid }}</td>
          <td>{{ item.SvrSrt }}</td>
          <td>{{ item.seid }}</td>
          <td>{{ item.SRT }}</td>
          <td>{{ item.name }}</td>
          <td>{{ item.deviceAddress }}</td>
          <td>{{ item.deviceHostName }}</td>
          <td>{{ item.severity }}</td>
          <td>{{ item.destinationAddress }}</td>
          <td>{{ item.destinationUserName }}</td>
          <td>{{ item.sourceAddress }}</td>
          <td>{{ item.sourceUserName }}</td>
          <td>{{ item.deviceVendor }}</td>
          <td>{{ item.deviceProduct }}</td>
          <td>{{ item.agentAddress }}</td>
        </tr>
      </tbody>
    </table>
    <button @click="loadMoreData">Load More Data</button>
  </div>
</template>

<script>
import * as data from '../assets/1.json';

export default {
  data() {
    return {
      tableData: [],
      jsonData: [],
      batchSize: 2,
      currentIndex: 0,
      searchText: '', 
    };
  },
  mounted() {
    this.loadData();
  },
  methods: {
    async loadData() {
      if (this.currentIndex < data.length) {
        const end = this.currentIndex + this.batchSize;
        this.tableData = this.tableData.concat(data.slice(this.currentIndex, end));
        this.currentIndex = end;
      }
    },
    loadMoreData() {
      this.loadData();
    },
    highlightText(text) {
      if (this.searchText && text) {
        const regex = new RegExp(this.searchText, 'gi');
        return text.replace(regex, '<span class="highlighted">$&</span>');
      }
      return text;
    },
    isItemHighlighted(item) {
      const itemText = Object.values(item).join(' ').toLowerCase();
      return itemText.includes(this.searchText.toLowerCase());
    },
    
  },
};
</script>

<style scoped>
.table-container {
  max-width: 700px;
  max-height: 400px;
  margin: 0;
}
.highlighted {
  background-color: yellow;
}
.custom-table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
}

.custom-table th {
  background-color: #333;
  color: white;
  font-weight: bold;
  padding: 0px;
}

.custom-table td {
  border: 1px solid #ddd;
  padding: 8px;
}

.custom-table tr:nth-child(even) {
  background-color: #f2f2f2;
}

button {
  background-color: #333;
  color: white;
  padding: 8px 16px;
  border: none;
  cursor: pointer;
  margin-top: 10px;
  border-radius: 5px;
}

button:hover {
  background-color: #555;
}
</style>
