<template>
  <div id="app" class="container">
    <div class="row mt-5" v-if="logDay.length > 0">
      <div class="col col-xs-12 col-sm-12 col-md-12 col-lg-12" align="center">
        <p class="mb-5">DASHBOARD DE LOGÍSTICA</p>
        <line-chart
          :chartData="logDay"
          :options="chartOptions"
          label="merged"
        ></line-chart>
      </div>
    </div>
    <Cards :cardInfos="cardInfos" />
  </div>
</template>

<script>
import axios from "axios";

import LineChart from "./components/LineChart";
import Cards from "./components/Cards";

export default {
  name: "App",
  components: {
    LineChart,
    Cards,
  },
  data() {
    return {
      infoTraveledDay: [],
      totalKmTraveled: [],
      totalFinesDay: [],
      totalFines: [],
      logDay: [],
      cardInfos: [],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
  async created() {
    const { data } = await axios.get(
      "https://6050b01f5346090017670430.mockapi.io/api/routes"
    );

    // Merge information from the same day into a single object
    this.infoTraveledDay = Object.values(
      data.reduce((a, { date, initialKm, finalKm }) => {
        a[date] = a[date] || {
          date,
          initialKm: 0,
          finalKm: 0,
          traveledKm: 0,
          averageTraveld: 0,
        };
        a[date].finalKm = Number(a[date].finalKm) + Number(finalKm);
        a[date].initialKm = Number(a[date].initialKm) + Number(initialKm);
        a[date].traveledKm = a[date].finalKm - a[date].initialKm;
        a[date].averageTraveld = (a[date].finalKm - a[date].initialKm) / 2;
        return a;
      }, {}),
    );

    // Generates array of fines for each day
    this.totalFinesDay = Object.values(
      data.reduce((a, { date, finesTotalAmount }) => {
        a[date] = a[date] || { date, finesTotalAmount: 0 };
        a[date].finesTotalAmount =
          Number(a[date].finesTotalAmount) + Number(finesTotalAmount);

        return a;
      }, {}),
    );

    // Generates variable with the Total of all fines in the period
    this.totalFines = this.totalFinesDay.reduce(
      (prev, cur) => prev + cur.finesTotalAmount,
      0
    );

    // Function to merge two arrays in object
    function mergeArrayObjects(infoTraveledDay, totalFinesDay) {
      return infoTraveledDay.map((item, i) => {
        if (item.id === totalFinesDay[i].id) {
          return Object.assign({}, item, totalFinesDay[i]);
        };
      })
    };

    this.logDay = mergeArrayObjects(this.infoTraveledDay, this.totalFinesDay);

    // Generates variable with the Total kilometers traveled during the period
    this.totalKmTraveled = this.infoTraveledDay.reduce(
      (prev, cur) => prev + cur.traveledKm,
      0
    );

    // Constructor function to create one array  
    function CardInfo(totalFines, totalKmTraveled) {
      this.totalFines = totalFines;
      this.totalKmTraveled = totalKmTraveled;
    };

    this.cardInfos = new CardInfo(this.totalFines, this.totalKmTraveled);
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Antonio&display=swap");

p {
  font-family: "Antonio", sans-serif;
  font-size: 64px;
}
</style>
