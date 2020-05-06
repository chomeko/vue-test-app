<template>
  <dev id="employee-form">
    <form @submit.prevent="handleSubmit">
      <label>Employee name</label>
      <input v-model="employee.name" type="text" />
      <label>Employee Email</label>
      <input v-model="employee.email" type="text" />
      <button>Add Employee</button>
    </form>
  </dev>
</template>

<script>
export default {
  name: 'employee-form',
  data(){
    return {
      employee: {
        name: '',
        email: '',
      }
    }
  },
  methods: {
    handleSubmit(){
      this.$emit('add:employee', this.employee)
    }
  },
}
</script>

<style scoped>
  form {
    margin-bottom: 2rem;
  }
  label {
    font-weight: bold;
    font-size: 14px;
  }
  input {
    display: block;
    width: 100%;
    height: 30px;
    margin: 10px 0;
    font-size: 14px;
    border: 1px solid #555;
    border-radius: 5px;
  }
  button {
    padding: 10px 20px;
    font-size: 16px;
    color: #ffffff;
    border-radius: 5px;
    box-shadow: 1px 2px 2px 1px rgb(68, 68, 68);
    transition: .1s;
  }
  button:hover {
    box-shadow: 0px 0px 1px rgb(68, 68, 68);
  }
</style>>