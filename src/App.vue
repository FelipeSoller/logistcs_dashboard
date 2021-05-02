<template>
  <div id="app" class="container">
    <div class="row mt-5" v-if="mergedArray.length > 0">
      <div class="col col-xs-12 col-sm-12 col-md-12 col-lg-12"  align="center">
        <h2 class="mb-5">DASHBOARD DE LOGÍSTICA</h2>
        <line-chart :chartData="mergedArray" :options="chartOptions" label="merged"></line-chart>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

import LineChart from './components/LineChart'

export default {
  name: "App",
  components: {
    LineChart,
  },
  data() {
    return {
      totalKmTraveled: [],
      totalFines: [],
      mergedArray: [],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      },
    };
  },
  async created() {
    const { data } = await axios.get(
      "https://6050b01f5346090017670430.mockapi.io/api/routes"
    );

    this.totalKmTraveled = Object.values(
      data.reduce((a, { date, initialKm, finalKm }) => {
        a[date] = a[date] || { date, initialKm: 0, finalKm: 0, traveledKm: 0, averageTraveld: 0 };
        a[date].finalKm = Number(a[date].finalKm) + Number(finalKm);
        a[date].initialKm = Number(a[date].initialKm) + Number(initialKm);
        a[date].traveledKm = a[date].finalKm - a[date].initialKm;
        a[date].averageTraveld = (a[date].finalKm - a[date].initialKm)/2;
        return a;
      }, {})
    );

    this.totalFines = Object.values(
      data.reduce((a, { date, finesTotalAmount }) => {
        a[date] = a[date] || { date, finesTotalAmount: 0 };
        a[date].finesTotalAmount =
          Number(a[date].finesTotalAmount) + Number(finesTotalAmount);

        return a;
      }, {})
    );

    function mergeArrayObjects(totalKmTraveled, totalFines) {
      return totalKmTraveled.map((item, i) => {
        if (item.id === totalFines[i].id) {
          return Object.assign({}, item, totalFines[i]);
        }
      });
    }

    this.mergedArray = mergeArrayObjects(this.totalKmTraveled, this.totalFines);
    console.log(this.mergedArray);
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Antonio&display=swap');

h2 {
  font-family: 'Antonio', sans-serif;
}

</style>
