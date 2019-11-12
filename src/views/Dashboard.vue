<template>
  <v-container>
    <h1>Dashboard</h1>

    <v-row>
      <v-col v-for="sale in sales" :key="`${sale.title}`">
        <SalesGraph :sale="sale"></SalesGraph>
      </v-col>
    </v-row>

    <v-row>
      <v-col v-for="stat in statistics" :key="`${stat.title}`">
        <StatisticCard :statistic="stat"></StatisticCard>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="8">
        <EmployeesTable
          :employees="employees"
          @select-employee="setEmployee"
        ></EmployeesTable>
      </v-col>

      <v-col cols="4">
        <EventTimeline :timeline="timeline"></EventTimeline>
      </v-col>
    </v-row>

    <v-snackbar v-model="snackbar">
      {{ selectedEmployee.name }} - {{ selectedEmployee.title }}
      <v-btn color="pink" text @click="snackbar = false">
        Close
      </v-btn>
    </v-snackbar>
  </v-container>
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
