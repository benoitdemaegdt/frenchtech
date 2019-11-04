<template>
  <div>
    <div class="companyList" v-for="(company, index) in companies" :key="company.name">
        <a :href="company.url" target="_blank">{{company.name}}</a><span v-show="index!==companies.length-1"> | </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'List',
  props: {
    companies: {
      type: Array,
      required: true,
    },
  },
}
</script>

<style scoped>
.companyList {
  display:inline;
}
</style>