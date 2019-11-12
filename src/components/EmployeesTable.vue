<template>
  <v-data-table
    :headers="headers"
    :items="employees"
    :items-per-page="5"
    :multi-sort="true"
    class="elevation-1"
    @click:row="selectRow"
  ></v-data-table>
</template>

<script>
export default {
  name: 'EmployeesTable',
  props: {
    employees: {
      type: Array,
      required: true
    }
  },
  data: () => ({
    headers: [
      {
        text: 'EmployeeID',
        value: 'id'
      },
      {
        text: 'Name',
        value: 'name'
      },
      {
        text: 'Position Title',
        value: 'title'
      },
      {
        text: 'Salary ($)',
        value: 'salary'
      }
    ]
  }),
  methods: {
    selectRow(evt) {
      const employee = {
        name: evt.name,
        title: evt.title
      }

      this.$emit('select-employee', employee)
    }
  }
}
</script>
