<template>
 
  <div>
  <h2>Register</h2>
    <div class="card mx-xl-5">
    <div class="card-body">
      <b-form class="m-5" @submit="onSubmit" v-if="!suscesfullyCreated">
        <b-container class="bv-example-row">

          <b-row>
            <b-col>
              <b-form-input
                v-model="form.name"
                type="text"
                required
                placeholder="Enter name"
                class="m-1"
                :value="this.form.name"
              ></b-form-input>
            </b-col>
          </b-row>
          
          <b-row>
            <b-col>
              <b-form-input
                v-model="form.lastname"
                type="text"
                required
                placeholder="Enter last name"
                class="m-1"
                :value="this.form.lastname"
              ></b-form-input>
            </b-col>
          </b-row>

          <b-row>
            <b-col>
              <b-form-input
                v-model="form.nick"
                type="text"
                required
                placeholder="Enter nick"
                class="m-1"
                :value="this.form.nick"
                :state="this.validation.errorNick"
                aria-describedby="validationNick"
              ></b-form-input>
              <b-form-invalid-feedback id="validationNick">
                This nick is already taken
              </b-form-invalid-feedback>
            </b-col>
          </b-row>
          <b-row>
            <b-col>
              <b-form-input
                v-model="form.email"
                type="email"
                required
                placeholder="Enter email"
                class="m-1"
                :value="this.form.email"
                :state="this.validation.errorEmail"
                aria-describedby="validationEmail"
              ></b-form-input>
              <b-form-invalid-feedback id="validationEmail">
                This email already exist. Please choose another. 
              </b-form-invalid-feedback>
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
              ></b-form-input>
            </b-col>
          </b-row>

          <b-row>
            <b-col>
              <b-form-select
                id='listCountryElement'
                v-model="form.country"
                :options="countriesList"
                required>
                <template v-slot:first>
                  <option :value="null" disabled>-- Please select your country --</option>
                </template>
              </b-form-select>
            </b-col>
          </b-row>
          <b-row class="m-5">
            <b-col>
              <b-button class="m-1" type="submit" variant="primary">Submit</b-button>
            </b-col>
            <b-col>
              <b-button class="m-1" type="button" variant="danger">Reset password</b-button>
            </b-col>
          </b-row>
        </b-container>

      </b-form>
      <b-alert v-if="suscesfullyCreated" variant="success" show>Suscesfully created</b-alert>
      <b-button v-if="suscesfullyCreated" class="m-1" v-on:click="openJoinSession" variant="primary">Join Session</b-button>

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
        validation:{
          errorEmail:null,
          errorNick:null,
          suscesfullyCreated:false
        },
        form: {
          name: '',
          lastname: '',
          nick:'',
          email:'',
          password:'', 
          country: null
        },
        countriesList: ['Afghanistan','Albania','Algeria','Andorra','Angola','Antigua and Barbuda','Argentina','Armenia','Australia','Austria','Azerbaijan','Bahamas','Bahrain','Bangladesh','Barbados','Belarus','Belgium','Belize','Benin','Bhutan','Bolivia','Bosnia and Herzegovina','Botswana','Brazil','Brunei','Bulgaria','Burkina Faso','Burundi'
            ,'Cabo Verde','Cambodia','Cameroon','Canada','Central African Republic (CAR)','Chad','Chile','China','Colombia','Comoros','Congo, Democratic Republic of the','Congo, Republic of the','Costa Rica','Cote d´Ivoire','Croatia','Cuba','Cyprus','Czechia','Denmark','Djibouti','Dominica','Dominican Republic','Ecuador','Egypt','El Salvador','Equatorial Guinea','Eritrea','Estonia'
            ,'Eswatini (formerly Swaziland)','Ethiopia','Fiji','Finland','France','Gabon','Gambia','Georgia','Germany','Ghana','Greece','Grenada','Guatemala','Guinea','Guinea-Bissau','Guyana','Haiti','Honduras','Hungary','Iceland','India','Indonesia','Iran','Iraq','Ireland','Israel','Italy','Jamaica'
            ,'Japan','Jordan','Kazakhstan','Kenya','Kiribati','Kosovo','Kuwait','Kyrgyzstan','Laos','Latvia','Lebanon','Lesotho','Liberia','Libya','Liechtenstein','Lithuania','Luxembourg','Madagascar','Malawi','Malaysia','Maldives','Mali','Malta','Marshall Islands','Mauritania','Mauritius','Mexico','Micronesia','Moldova'
            ,'Monaco','Mongolia','Montenegro','Morocco','Mozambique','Myanmar (formerly Burma)','Namibia','Nauru','Nepal','Netherlands','New Zealand','Nicaragua','Niger','Nigeria','North Korea','North Macedonia (formerly Macedonia)','Norway','Oman','Pakistan','Palau','Palestine','Panama','Papua New Guinea','Paraguay','Peru','Philippines','Poland'
            ,'Portugal','Qatar','Romania','Russia','Rwanda','Saint Kitts and Nevis','Saint Lucia','Saint Vincent and the Grenadines','Samoa','San Marino','Sao Tome and Principe','Saudi Arabia','Senegal','Serbia','Seychelles','Sierra Leone','Singapore','Slovakia','Slovenia','Solomon Islands','Somalia','South Africa','South Korea','South Sudan','Spain','Sri Lanka','Sudan','Suriname','Sweden'
            ,'Switzerland','Syria','Taiwan','Tajikistan','Tanzania','Thailand','Timor-Leste','Togo','Tonga','Trinidad and Tobago','Tunisia','Turkey','Turkmenistan','Tuvalu','Uganda','Ukraine','United Arab Emirates (UAE)','United Kingdom (UK)','United States of America (USA)','Uruguay','Uzbekistan','Vanuatu','Vatican City (Holy See)','Venezuela','Vietnam','Yemen','Zambia','Zimbabwe']
      }
    },
    methods: {
      openJoinSession(){
        this.$emit('abrirInicioSesion');
      },
      onSubmit(evt) {
        //this.form.country = document.getElementById('listCountryElement').value;
        evt.preventDefault();
        // You should have a server side REST API 
        axios.post('http://localhost:8020/registerSession', JSON.stringify(this.form), {
            'Content-type': 'multipart/form-data',
            'Access-Control-Allow-Origin': '*'
        }).then(resp=>{
          ////console.log(resp.data.result != 'ko');
          //if(resp.data.result !== 'ko'){
            // emitimos el id a la pantalla principal
            //this.$emit('registeredSession', resp.data.result );
          //}
          
          if(resp.data.result == "OK"){
            this.suscesfullyCreated = true;
            this.$forceUpdate();
          }else if(resp.data.result == "KO"){
            this.validation.errorEmail = !resp.data.errorFields.includes("email");
            this.validation.errorNick  = !resp.data.errorFields.includes("nick");
          }
        }).catch();
      }
    },

  }
</script>
