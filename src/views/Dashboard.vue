<template>
  <div>
    <h1>Dashboard</h1>

    <SalesGraph
      v-for="sale in sales"
      :key="`${sale.title}`"
      :sale="sale"
    ></SalesGraph>

    <StatisticCard
      v-for="stat in statistics"
      :key="`${stat.title}`"
      :statistic="stat"
    ></StatisticCard>

    <EmployeesTable
      :employees="employees"
      @selectEmployee="setEmployee"
    ></EmployeesTable>

    <EventTimeline :timeline="timeline"></EventTimeline>

    <v-snackbar v-model="snackbar">
      {{ selectedEmployee.name }} - {{ selectedEmployee.title }}
      <v-btn color="pink" text @click="snackbar = false">
        Close
      </v-btn>
    </v-snackbar>
  </div>
</template>

<script>
import SalesGraph from '@/components/SalesGraph'
import EmployeesTable from '@/components/EmployeesTable'
import StatisticCard from '@/components/StatisticCard'
import EventTimeline from '@/components/EventTimeline'

export default {
  name: 'Dashboard',
  data: () => ({
    selectedEmployee: {
      name: '',
      title: ''
    },
    snackbar: false,
    employees: [],
    sales: [],
    statistics: [],
    timeline: []
  }),
  components: {
    SalesGraph,
    EmployeesTable,
    StatisticCard,
    EventTimeline
  },
  methods: {
    fetchData(pathName) {
      fetch(`http://localhost:3000/${pathName}`, {
        method: 'GET'
      })
        .then(response => {
          return response.json()
        })
        .then(data => {
          this[pathName] = data
        })
    },
    setEmployee(evt) {
      this.snackbar = true
      this.selectedEmployee.name = evt.name
      this.selectedEmployee.title = evt.title
    }
  },
  created() {
    this.fetchData('employees')
    this.fetchData('sales')
    this.fetchData('statistics')
    this.fetchData('timeline')
  }
}
</script>
