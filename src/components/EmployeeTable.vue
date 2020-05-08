<template lang="pug">
  #employee-table
    p.empty-table(v-if="employees.length < 1")
      | No employees
    table
      thead
        tr
          th Employee name
          th Employee email
          th Actions
          th Day
      tbody
        tr(v-for="employee in employees" v-bind:key="employee.id")
          td(v-if="editing === employee.id")
            input(type="text" v-model="employee.name")
          td(v-else="") {{ employee.name }}
          td(v-if="editing === employee.id")
            input(type="text" v-model="employee.email")
          td(v-else="") {{ employee.email }}
          td(v-if="editing === employee.id")
            button(@click="editEmployee(employee)") Save
            button.muted-button(@click="editing = null") Cancel
          td(v-else="")
            button(@click="editMode(employee.id)") Edit
            button(@click="$emit('delete:employee', employee.id)") Delete
          td {{ employee.day }}
</template>

<script>
  export default {
    name: 'employee-table',
    props: {
      employees: Array,
    },
    data() {
      return {
        editing: null,
      }
    },
    methods: {
      editMode(id) {
        this.editing = id
      },
      editEmployee(employee) {
        if (employee.name === '' || employee.email === '') return
        this.$emit('edit:employee', employee.id, employee)
        this.editing = null
      }
    }
  }
</script>

<style scoped lang="sass">
  th,td
    padding: 10px
    width: 340px
    text-align-last: left
    border-bottom: 1px solid #000

  button
    margin: 0 0.5rem 0 0
</style>