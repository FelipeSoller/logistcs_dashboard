<script>
import { Bar, Line } from "vue-chartjs";

export default {
  extends: Bar, Line,
  props: {
    label: {
      type: String,
    },
    chartData: {
      type: Array,
    },
    options: {
      type: Object,
    },
  },
  mounted() {
    const date = this.chartData.map((d) => d.date);
    const traveledKm = this.chartData.map((d) => d.traveledKm);
    const averageTraveld = this.chartData.map((d) => d.averageTraveld);

    this.renderChart(
      {
        labels: date,
        datasets: [
          {
            label: "Quilômetros rodados totais por dia",
            data: traveledKm,
            backgroundColor: [
              "rgba(255, 0, 0, 0.2)",
              "rgba(255, 128, 0, 0.2)",
              "rgba(255, 255, 0, 0.2)",
              "rgba(250, 7, 61, 0.2)",
              "rgba(128, 255, 0, 0.2)",
              "rgba(0, 255, 0, 0.2)",
              "rgba(0, 255, 128, 0.2)",
              "rgba(0, 255, 255, 0.2)",
              "rgba(0, 128, 255, 0.2)",
              "rgba(0, 0, 255, 0.2)",
              "rgba(128, 0, 255, 0.2)",
              "rgba(255, 0, 255, 0.2)",
              "rgba(255, 0, 128, 0.2)",
              "rgba(255, 0, 64, 0.2)",
              "rgba(255, 0, 0, 0.2)",
            ],
            borderColor: [
              "rgb(255, 0, 0)",
              "rgb(255, 128, 0)",
              "rgb(255, 255, 0)",
              "rgb(250, 7, 61)",
              "rgb(128, 255, 0)",
              "rgb(0, 255, 0)",
              "rgb(0, 255, 128)",
              "rgb(0, 255, 255)",
              "rgb(0, 128, 255)",
              "rgb(0, 0, 255)",
              "rgb(128, 0, 255)",
              "rgb(255, 0, 255)",
              "rgb(255, 0, 128)",
              "rgb(255, 0, 64)",
              "rgb(255, 0, 0)",
            ],
            barThickness: 40
          },
          {
            label: "Média de quilômetros rodados totais no dia",
            type: "line",
            data: averageTraveld,
          }
        ],
      },
      this.options
    );
  },
};
</script>
