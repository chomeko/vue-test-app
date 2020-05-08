<template lang="pug">
  dev#employee-form
    form(@submit.prevent="handleSubmit")
      label Day
      input(
        type="date"
        :class="{ 'hass-error': submitting && invalidDay }"
        v-model="employee.day"
        @focus="clearStatus"
        ref="first"
      )
      label Employee name
      input(
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="employee.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      )
      label Employee Email
      input(
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        v-model="employee.email"
        @focus="clearStatus"
        @keypress="clearStatus"
      )
      label Image File
      input(
        type="file"
        class="input_image"
        @change="onDrop($event)"
      )
      <img :src="data.image">
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
      data: {
        image: null,
      }
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
    //onSubmit:function () {
//
    //  let data = new FormData();
    //  data.append('files[]',this.files);
//
    //  axios
    //    .post('/file',data)
    //    .then(function(response) {
    //      console.log(response.data);
    //    })
    //    .catch(function(error) {
    //      console.log(error);
    //    })
    //},
    onDrop(event) {
      const file = (event.target.files || event.dataTransfer)[0]
      if (file.type.startsWith("image/")) {
        this.data.image = window.URL.createObjectURL(file);
        this.data.name = file.name;
        this.data.type = file.type;
      }
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

  .input_image
    border: none
    width: 250px
    height: 20px

  button
    padding: 10px 20px
    font-size: 16px
    color: #ffffff
    border-radius: 5px
    box-shadow: 1px 2px 2px 1px rgb(68, 68, 68)
    transition: .1s

  button:hover
    box-shadow: 0px 0px 1px rgb(68, 68, 68)

  img
    max-width: 100%
    height: auto
    object-fit: cover

</style>>