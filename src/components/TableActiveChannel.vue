<template>
  <div class="table-container">
    <input v-model="searchText" placeholder="Поиск..." class="search-input" />
    <div class="scroll-table">
      <table class="custom-table">
        <thead>
          <tr>
            <th
              v-for="(column, index) in tableColumns"
              :key="index"
              :style="{ width: column.width }"
              @click="sortTable(column.field)"
            >
              {{ column.label }}
              <span v-if="column.field === sortBy">
                {{ sortOrder === 'asc' ? '▲' : '▼' }}
              </span>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in filteredTableData" :key="item.SvrSeid" :class="{ 'highlighted-row': isItemHighlighted(item) }">
            <td v-for="(column, index) in tableColumns" :key="index">
              <span v-if="item[column.field]" v-html="highlightText(item[column.field])"></span>
              <span v-else>{{ item[column.field] }}</span>
            </td>

          </tr>
        </tbody>
      </table>
    </div>
    <button @click="loadMoreData">Load More Data</button>
  </div>
</template>

<script>
import * as data from "../assets/data-active-table.json";

export default {
  data() {
    return {
      allTableData: null,
      tableData: [],
      batchSize: 700, // Кол-во строк
      currentIndex: 0,
      searchText: "",
      tableColumns: [
        { label: "SRT", field: "SRT", width: "5%" },
        { label: "Name", field: "name", width: "25%" },
        { label: "Severity", field: "severity", width: "5%" },
        { label: "SvrSeid", field: "SvrSeid", width: "10%" },
        { label: "SvrSrt", field: "SvrSrt", width: "5%" },
        { label: "Seid", field: "seid", width: "5%" },
        { label: "DeviceAddress", field: "deviceAddress", width: "10%" },
        { label: "DeviceHostName", field: "deviceHostName", width: "10%" },
        { label: "DestinationAddress", field: "destinationAddress", width: "10%" },
        { label: "DestinationUserName", field: "destinationUserName", width: "2%" },
        { label: "SourceAddress", field: "sourceAddress", width: "10%" },
        { label: "SourceUserName", field: "sourceUserName", width: "2%" },
        { label: "DeviceVendor", field: "deviceVendor", width: "10%" },
        { label: "DeviceProduct", field: "deviceProduct", width: "10%" },
        { label: "AgentAddress", field: "agentAddress", width: "10%" },
      ],
      sortBy: "", // Для сортировки
      sortOrder: "asc", // Направление сортировки
    };
  },
  mounted() {
    this.loadData();
  },
  computed: {
    filteredTableData() {
      return this.allTableData
        ? this.allTableData.filter((item) => this.isItemHighlighted(item))
        : [];
    },
  },
  methods: {

    // Временное решение, надо доработать server.py
    async loadData() {
      if (!this.allTableData) {
        this.allTableData = [];
      }

      if (this.currentIndex < data.length) {
        const end = this.currentIndex + this.batchSize;
        this.allTableData = this.allTableData.concat(data.slice(this.currentIndex, end));
        this.tableData = this.allTableData;
        this.currentIndex = end;
      }
    },
    highlightText(text) {
      if (typeof text !== 'string') {
        return text; // Return text as is if it's not a string
      }

      if (!this.searchText) {
        return text;
      }

      // Create a regular expression to search for text
      const regex = new RegExp(this.searchText, 'gi');

      // Replace the found text with a version wrapped in span tags for highlighting
      const highlightedText = text.replace(
        regex,
        (match) => `<span style="background-color: #f1ef66; color: #363638">${match}</span>`
      );

      return highlightedText;
    },

    loadMoreData() {
      this.loadData();
    },
    
    isItemHighlighted(item) {
      const itemText = Object.values(item).join(" ").toLowerCase();
      return itemText.includes(this.searchText.toLowerCase());
    },
    sortTable(field) {
      if (field === this.sortBy) {
        this.sortOrder = this.sortOrder === "asc" ? "desc" : "asc";
      } else {
        this.sortBy = field;
        this.sortOrder = "asc";
      }
      this.allTableData && this.allTableData.sort(this.sortFunction);
    },
    sortFunction(a, b) {
      const fieldA = a[this.sortBy];
      const fieldB = b[this.sortBy];
      if (this.sortOrder === "asc") {
        return fieldA > fieldB ? 1 : -1;
      } else {
        return fieldA < fieldB ? 1 : -1;
      }
    },
  },
};
</script>

<style scoped>
.custom-table thead {
  position: sticky;
  top: 0;
  background-color: #d8d8d8;
  z-index: 1;
}
.table-container {
  max-width: 100%;
  border: 1px solid #363638;
  margin: 0;
}

.custom-table {
  
  border-collapse: collapse;
  text-align: left;
}

.custom-table th {
  background-color: #343338;
  font-weight: bold;
  padding: 10px 15px;
  border: 1px solid #363638;
  border-top-width: 0px;
  color: #fff;
}

.custom-table td {
  border: 1px solid #363638;
  padding: 5px 5px;
  font-size: 14px;
  color: #fff;
}

.custom-table tr:nth-child(even) {
  background-color: #262628
}

.scroll-table {
  height: 650px;
  overflow-x: auto;
  width: 100%;
  margin-top: 0px;
  margin-bottom: 20px;
  border-bottom: 1px solid #000000;
}


/* Стили для скролла */
::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  box-shadow: inset 0 0 6px rgba(255, 0, 0, 0.3);
}

::-webkit-scrollbar-thumb {
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
}

button {
  background-color: #333;
  color: white;
  padding: 8px 16px;
  border: none;
  cursor: pointer;

  border-radius: 5px;
}

input {
  margin-bottom: 20px;
  height: 20px;
}

button:hover {
  background-color: #555;
}
</style>
