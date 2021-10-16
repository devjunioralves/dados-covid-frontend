
<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col">
        <h1 class="text-center">Dados COVID-19 SC</h1>
      </div>
    </div>
    <div class="row mt-5" v-if="arrConfirmed.length > 0">
      <div class="col">
        <h2 class="text-center">Positivo</h2>
        <line-chart
          :chartData="arrConfirmed"
          :options="chartOptions"
          :chartColors="arrConfirmedChartColors"
          label="Positivo"
        />
      </div>
    </div>

    <div class="row mt-5" v-if="arrEstimatedPopulation.length > 0">
      <div class="col">
        <h2 class="text-center">População estimada</h2>
        <line-chart
          :chartData="arrEstimatedPopulation"
          :options="chartOptions"
          :chartColors="arrEstimatedPopulationChartColors"
          label="População estimada"
        />
      </div>
    </div>

    <div class="row mt-5" v-if="arrDeaths.length > 0">
      <div class="col">
        <h2 class="text-center">Mortes</h2>
        <line-chart
          :chartData="arrDeaths"
          :options="chartOptions"
          :chartColors="arrDeathsChartColors"
          label="Mortes"
        />
      </div>
    </div>

    <div class="row mt-5" v-if="arrDeathRate.length > 0">
      <div class="col">
        <h2 class="text-center">Com respiradores</h2>
        <line-chart
          :chartData="arrDeathRate"
          :options="chartOptions"
          :chartColors="arrDeathRateChartColors"
          label="Com respiradores"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
import LineChart from "./components/LineChart.vue";
export default {
  components: {
    LineChart
  },
  data() {
    return {
      arrConfirmed: [],
      arrConfirmedChartColors: {
        borderColor: "#077187",
        pointBorderColor: "#0E1428",
        pointBackgroundColor: "#AFD6AC",
        backgroundColor: "#74A57F"
      },
      arrEstimatedPopulation: [],
      arrEstimatedPopulationChartColors: {
        borderColor: "#251F47",
        pointBorderColor: "#260F26",
        pointBackgroundColor: "#858EAB",
        backgroundColor: "#858EAB"
      },
      arrDeaths: [],
      arrDeathsChartColors: {
        borderColor: "#190B28",
        pointBorderColor: "#190B28",
        pointBackgroundColor: "#E55381",
        backgroundColor: "#E55381"
      },
      arrDeathRate: [],
      arrDeathRateChartColors: {
        borderColor: "#784F41",
        pointBorderColor: "#784F41",
        pointBackgroundColor: "#BBE5ED",
        backgroundColor: "#BBE5ED"
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      }
    };
  },
  async created() {
    const { data } = await axios.get("http://localhost:8000/infosCovid/");
    const results = data.message.results
    results.forEach(d => {
      const date = moment(d.date, "YYYYMMDD").format("MM/DD");
      const {
        confirmed,
        estimated_population,
        deaths,
        death_rate,
        city
      } = d;
      this.arrConfirmed.push({ date, total: confirmed, cidade: city });
      this.arrEstimatedPopulation.push({ date, total: estimated_population, cidade: city });
      this.arrDeaths.push({ date, total: deaths, cidade: city });
      this.arrDeathRate.push({ date, total: death_rate, cidade: city });
    });
  }
};
</script>