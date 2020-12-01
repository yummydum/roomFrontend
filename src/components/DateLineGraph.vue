<template>
  <div>
    <datepicker
      :language="ja"
      :value="state.startDate"
      @input="setStartDate($event)"
    ></datepicker>
    <datepicker
      :language="ja"
      :value="state.endDate"
      @input="setEndDate($event)"
    ></datepicker>
    <line-graph :chart-data="datacollection" :options="options" />
  </div>
</template>

<script>
import Datepicker from "vuejs-datepicker";
import LineGraph from "./LineGraph.vue";
import { ja } from "vuejs-datepicker/dist/locale";
import axios from "axios";

// Init dates
var endDate = new Date();
var startDate = new Date();
startDate.setDate(endDate.getDate() - 1);
const state = {
  startDate: startDate,
  endDate: endDate,
};

export default {
  name: "DateLineGraph",
  props: ["measure"],
  components: {
    LineGraph,
    Datepicker,
  },
  data() {
    return {
      ja: ja,
      datacollection: {},
      options: { responsive: true, maintainAspectRatio: false },
      state: state,
    };
  },

  created() {
    this.fetchData();
  },

  methods: {
    fetchData() {
      // Format dates
      let d1 = this.state.startDate;
      console.log(d1);
      let s = `${d1.getFullYear()}-${d1.getMonth() + 1}-${d1.getDate() + 1}`;
      let d2 = this.state.endDate;
      let e = `${d2.getFullYear()}-${d2.getMonth() + 1}-${d2.getDate() + 1}`;

      // Fetch data
      let q = `http://localhost:5000/data?start_date=${s}&end_date=${e}&measure=${this.measure}`;
      console.log(q);
      axios
        .get(q)
        .then((response) => {
          this.datacollection = {
            labels: response.data["date"],
            datasets: [
              {
                label: this.measure,
                data: response.data[this.measure],
              },
            ],
          };
        })
        .catch((error) => console.error(error));
    },
    setStartDate(d) {
      this.state.startDate = d;
      this.fetchData();
    },
    setEndDate(d) {
      this.state.endDate = d;
      this.fetchData();
    },
  },
};
</script>
