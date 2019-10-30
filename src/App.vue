<template>
  <div id="app">
    <NavBar  v-on:abrirInicioSesion="mostrarInicioSesion" v-on:mostrarVideos="mostrarVideos"/>
    <img alt="Vue logo" src="./assets/logo.png">
    <input class="form-control" type="text" placeholder="Search" aria-label="Search" width="20px" />
    <!--<Slider id="videoSlider" v-if="!setSesion"/>-->
    <component :is="componentMode"></component>
        <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <!-- child events -->
  </div>

</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import Slider from './components/Slider.vue'
import NavBar from './components/NavBar.vue'
import Form from './components/Form.vue'



export default {
  name: 'app',
  data() {
    return {
      setSesion : false,
      componentMode: 'Slider'
    }
  },
  methods:{
    onLogin(idSession){
      alert(idSession);
    },
    mostrarInicioSesion(){
      this.componentMode = 'Form';
    },
    mostrarVideos(){
      this.componentMode = 'slider';
    }
  },
  components: {
    //HelloWorld,
    Slider,
    NavBar,
    Form
  }

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 6vmin;
}
.formulario{
  width: none;
}
#videoSlider{
  display: block;
}
</style>


