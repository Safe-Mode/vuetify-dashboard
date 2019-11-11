<template>
  <div>
    <h1>Dashboard</h1>
    <v-data-table
      :headers="headers"
      :items="employees"
      :items-per-page="5"
      :multi-sort="true"
      class="elevation-1"
      @click:row="selectRow"
    ></v-data-table>

    <v-snackbar v-model="snackbar">
      {{ currentItem.name }} - {{ currentItem.title }}
      <v-btn color="pink" text @click="snackbar = false">
        Close
      </v-btn>
    </v-snackbar>
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  data: () => ({
    currentItem: '',
    snackbar: false,
    headers: [
      {
        text: 'ID',
        align: 'left',
        sortable: false,
        value: 'id'
      },
      { text: 'Name', value: 'name' },
      { text: 'Title', value: 'title' },
      { text: 'Salary ($)', value: 'salary' }
    ],
    employees: []
  }),
  methods: {
    selectRow(evt) {
      this.snackbar = true
      this.currentItem = evt
    }
  },
  created() {
    fetch('http://localhost:3000/employees', {
      method: 'GET'
    })
      .then(response => {
        return response.json()
      })
      .then(data => {
        this.employees = data
      })
  }
}
</script>
