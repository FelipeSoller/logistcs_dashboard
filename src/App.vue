<template>
  <div id="app"></div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  components: {},
  data() {
    return {
      totalKmTravelled: [],
      totalFines: [],
      mergedArray: [],
    };
  },
  async created() {
    const { data } = await axios.get(
      "https://6050b01f5346090017670430.mockapi.io/api/routes"
    );

    this.totalKmTravelled = Object.values(
      data.reduce((a, { date, initialKm, finalKm }) => {
        a[date] = a[date] || { date, initialKm: 0, finalKm: 0, travelledKm: 0 };
        a[date].finalKm = Number(a[date].finalKm) + Number(finalKm);
        a[date].initialKm = Number(a[date].initialKm) + Number(initialKm);
        a[date].travelledKm = a[date].finalKm - a[date].initialKm;
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

    function mergeArrayObjects(totalKmTravelled, totalFines) {
      return totalKmTravelled.map((item, i) => {
        if (item.id === totalFines[i].id) {
          return Object.assign({}, item, totalFines[i]);
        }
      });
    }

    this.mergedArray = mergeArrayObjects(this.totalKmTravelled, this.totalFines);
  },
};
</script>

<style></style>
