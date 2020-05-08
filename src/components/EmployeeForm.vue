<template lang="pug">
  dev#employee-form
    form(@submit.prevent="handleSubmit")
      label Day
      input(type="date" :class="{ 'hass-error': submitting && invalidDay }" v-model="employee.day" @focus="clearStatus" ref="first")
      label Employee name
      input(type="text" :class="{ 'has-error': submitting && invalidName }" v-model="employee.name" @focus="clearStatus" @keypress="clearStatus")
      label Employee Email
      input(type="text" :class="{ 'has-error': submitting && invalidEmail }" v-model="employee.email" @focus="clearStatus" @keypress="clearStatus")
      p.error-message(v-if="error && submitting")
        | ❗Please fill out all required fields
      p.success-message(v-if="success")
        | ✅ Employee successfully added
      button Add Employee
</template>


<script>
export default {
  name: 'employee-form',
  data(){
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: '',
        email: '',
        day: '',
      },
    }
  },
  methods: {
    handleSubmit(){
      this.submitting = true
      this.clearStatus()

      if (this.invalidName || this.invalidEmail || this.invalidDay){
        this.error = true
        return
      }

      this.$emit('add:employee', this.employee)
      this.$refs.first.focus()
      this.employee = {
        name: '',
        email: '',
        day: '',
      }
      this.error = false
      this.success = true
      this.submitting = false
    },
    clearStatus(){
      this.success = false
      this.error = false
    },
  },
  computed: {
    invalidName(){
      return this.employee.name === ''
    },
    invalidEmail(){
      return this.employee.email === ''
    },
    invalidDay(){
      return this.employee.day === ''
    },
  },
}
</script>

<style scoped lang="sass">
  form
    margin-bottom: 2rem

  [class*='-message']
    font-weight: bold

  .error-message
    color: #d33c40

  .success-message
    color: #32a95d

  label
    font-weight: bold
    font-size: 14px

  input
    display: block
    width: 100%
    height: 30px
    margin: 10px 0
    font-size: 14px
    border: 1px solid #555
    border-radius: 5px

  button
    padding: 10px 20px
    font-size: 16px
    color: #ffffff
    border-radius: 5px
    box-shadow: 1px 2px 2px 1px rgb(68, 68, 68)
    transition: .1s

  button:hover
    box-shadow: 0px 0px 1px rgb(68, 68, 68)
</style>>