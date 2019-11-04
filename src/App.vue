<template>
  <div id="app">
    <h1>Analyse de la stack technique de quelques startups de la frenchtech</h1>
    <h2>Backend</h2>
    <bar-chart :chartData='backendData'></bar-chart>
    <h2>Frontend</h2>
    <bar-chart :chartData='frontendData'></bar-chart>
  </div>
</template>

<script>
import companies from './data/companies.json';
import BarChart from './components/BarChart.vue';

export default {
  name: 'app',
  components: {
    BarChart,
  },
  data() {
    return {
      backendData: this.formatBackendData(),
      frontendData: this.formatFrontendData(),
    };
  },
  methods: {
    formatBackendData() {
      let languages = {};
      companies.forEach((company) => {
        languages[company.stack.backend] = (languages[company.stack.backend] || 0) + 1;
      });
      languages = this.sortObjectByPropertyValue(languages);
      return {
        labels: Object.keys(languages),
        datasets: [
          {
            label: 'utilisations',
            backgroundColor: '#f87979',
            borderWidth: 1,
            data: Object.values(languages),
          },
        ],
      }
    },
    formatFrontendData() {
      let languages = {};
      companies.forEach((company) => {
        languages[company.stack.frontend] = (languages[company.stack.frontend] || 0) + 1;
      });
      languages = this.sortObjectByPropertyValue(languages);
      return {
        labels: Object.keys(languages),
        datasets: [
          {
            label: 'utilisations',
            backgroundColor: '#f87979',
            borderWidth: 1,
            data: Object.values(languages),
          },
        ],
      }
    },
    sortObjectByPropertyValue(inputObject) {
      let sortedObject = {};
      let sortedArray = [];
      for (var key in inputObject) {
        sortedArray.push([key, inputObject[key]]);
      }
      sortedArray.sort((a, b) => {
        return b[1] - a[1];
      });
      sortedArray.forEach((item) => {
        sortedObject[item[0]] = item[1];
      });
      return sortedObject;
    }
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
