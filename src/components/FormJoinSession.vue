<template>
  <div>
  <h1>JOIN SESSION</h1>
    <div class="card mx-xl-5">
    <div class="card-body">
      <b-form class="m-5" @submit="onSubmit">
        <b-container class="bv-example-row">
          <b-row>
            <b-col>
              <b-form-input
                v-model="form.email"
                type="email"
                required
                placeholder="Enter email"
                class="m-1"
                :value="this.form.email"
                :state="this.validation.userJoinned"
              ></b-form-input>
            </b-col>
          </b-row>
          <b-row>
            <b-col>
              <b-form-input
                v-model="form.password"
                required
                placeholder="Enter password"
                type="password"
                class="m-1"
                :value="this.form.password"
                :state="this.validation.userJoinned"
                aria-describedby="validationNick"
              ></b-form-input>

              <b-form-invalid-feedback id="validationNick">
                Error to join session, check the email or the password, there are incorrect.
              </b-form-invalid-feedback>
            </b-col>

          </b-row>
          <b-button class="m-5 w-25" type="submit" variant="primary">Submit</b-button>
          <b-button class="m-5 w-25" type="button" variant="danger">Reset password</b-button>
        </b-container>

      </b-form>
    </div>
  </div>
  </div>
</template>

<script>
  import axios from 'axios';

// controller
  export default {
    data() {
      return {
        form: {
          email: '',
          password: '',
          genero: null,
          checked: []
        },
        validation:{
          userJoinned: null
        },
        show: true
      }
    },
    methods: {
      onSubmit(evt) {
        evt.preventDefault();

        // You should have a server side REST API 
        axios.post('http://localhost:8020/joinSession', JSON.stringify(this.form), {
            'Content-type': 'multipart/form-data',
            'Access-Control-Allow-Origin': '*'
        }).then(resp=>{
          console.log(resp.data.result != 'ko');
          if(resp.data.result !== 'ko'){
            this.$emit('sessionJoined', resp.data.result );
          }else{
            this.validation.userJoinned = false;
          }
        }).catch(err=>{

        });
      },
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.password = ''
        this.form.genero = null
        this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    },
    created(){
      this.form.email = 'da@gmail.com';
      this.form.password = 'da';
    }
  }
</script>
