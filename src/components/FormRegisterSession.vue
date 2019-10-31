<template>
  <div>
  <h1>REGISTER</h1>
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group
        id="input-group-1"
        label="Email address:"
        label-for="input-1"
        description="We'll never share your email with anyone else."
      >
        <b-form-input
          id="input-1"
          v-model="form.email"
          type="email"
          required
          placeholder="Enter email"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="City:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.city"
          required
          placeholder="City"
          type="text"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="Name:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.name"
          required
          placeholder="Enter the name"
          type="text"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="LastName:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.lastname"
          required
          placeholder="Enter the LastName"
          type="text"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="Password:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.password"
          required
          placeholder="Enter password"
          type="password"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3" label="genero:" label-for="input-3">
        <b-form-select
          id="input-3"
          v-model="form.genero"
          :options="genero"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group id="input-group-4">
        <b-form-checkbox-group v-model="form.checked" id="checkboxes-4">
          <b-form-checkbox value="me">Check me out</b-form-checkbox>
          <b-form-checkbox value="that">Check that out</b-form-checkbox>
        </b-form-checkbox-group>
      </b-form-group>

      <b-button type="submit" variant="primary">Submit</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
  </div>
</template>

<script>
// controller
  export default {
    data() {
      return {
        form: {
          email: '',
          password: '',
          city:'',
          name:'',
          lastname:'', 
          genero: null,
          checked: []
        },
        genero: [{ text: 'Select One', value: null }, 'Mujer', 'Hombre', 'Otro'],
        show: true
      }
    },
    methods: {
      onSubmit(evt) {
        // se emite al padre el evento de clicked
        this.$emit('sesionIniciada', this.form.email);
        evt.preventDefault();
      },
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.password = ''
        this.form.genero = null
        this.form.checked = []
        this.form.name = ''
        this.form.lastname = ''
        this.form.city = ''
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
  }
</script>
