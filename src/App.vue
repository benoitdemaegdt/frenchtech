<template>
  <div id="app">
    <h1>Analyse de la stack technique de quelques startups de la frenchtech</h1>
    <p>Cette page vise à livrer quelques analyses portant sur des startups françaises à succès.</p>
    <img src='./assets/tech.jpg'>
    <h2>Échantillon</h2>
    <p>Les {{companies.length}} startups de cet échantillon ont été sélectionnées de manière arbitraire. Globalement, il s'agit d'entreprises innovantes, ayant un ADN "tech" qui ont levé beaucoup d'argent. L'échantillon d'analyse est ainsi composé des startups suivantes :</p>
    <list :companies="companies"></list>
    <h2>Source des données</h2>
    <p>Les données à l'origine des graphes présents sur cette page ont été collectées "à la main" en novembre 2019. Elles proviennent de différentes sources telles que : le site web des startups, leur blog technique, leur page welcome to the jungle, leurs offres d'emploi à destination des développeurs ...</p>
    <h2>Date de création</h2>
    <p>En quelle année ont été créées les startups françaises à succès ?</p>
    <line-chart :chartData='createdAtData' :styles="chartHeight"></line-chart>
    <h2>Backend</h2>
    <p>Quels sont les langages majoritairement utilisés côté serveur par les startups à succès ?</p>
    <bar-chart :chartData='backendData' :styles="chartHeight"></bar-chart>
    <h2>Frontend</h2>
    <p>Quels sont les langages majoritairement utilisés côté client par les startups à succès ?</p>
    <bar-chart :chartData='frontendData' :styles="chartHeight"></bar-chart>
  </div>
</template>

<script>
import companies from './data/companies.json';
import BarChart from './components/BarChart.vue';
import LineChart from './components/LineChart.vue';
import List from './components/List.vue';

export default {
  name: 'app',
  components: {
    BarChart,
    LineChart,
    List
  },
  mounted() {
    this.companies = companies;
  },
  data() {
    return {
      companies: [],
      createdAtData: this.formatCreatedAtData(),
      backendData: this.formatBackendData(),
      frontendData: this.formatFrontendData(),
    };
  },
  methods: {
    formatCreatedAtData() {
      let minYear = Math.min(...companies.map(company => company.createdAt));
      let maxYear = Math.max(...companies.map(company => company.createdAt));
      const labels = Array(maxYear - minYear + 1).fill().map((_, idx) => minYear + idx);
      const data = {};
      for (let label of labels) {
        data[label] = companies.filter(company => company.createdAt === label).length;
      }
      return {
        labels,
        datasets: [
          {
            label: 'créations',
            backgroundColor: '#f87979',
            pointBackgroundColor: 'white',
            borderWidth: 1,
            pointBorderColor: '#249EBF',
            data: Object.values(data),
          }
        ]
      }
    },
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
  },
  computed: {
    chartHeight() {
      return {
        height: '300px',
        position: 'relative'
      };
    },
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: auto;
  margin-top: 40px;
}

/* If the screen size is 601px wide or more, set max-width to 80%  */
@media screen and (min-width: 601px) {
  #app {
    max-width: 700px;
  }
  p {
    font-size: 21px;
  }
  a {
    font-size: 21px;
  }
}

/* If the screen size is 600px wide or less, set max-width to 100% */
@media screen and (max-width: 600px) {
  #app {
    max-width: 100%;
  }
  p {
    font-size: 18px;
  }
  a {
    font-size: 18px;
  }
}

a {
  color:black;
}
a:hover {
  color: #f87979;
}
p {
  text-align: left;
}
img {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 100%;
}

</style>
