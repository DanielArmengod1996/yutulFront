<template>
  <div id="app">
    
    <NavBar  
      v-on:abrirInicioSesion="mostrarInicioSesion" 
      v-on:mostrarVideos="mostrarVideos"
      v-on:abrirRegistroSesion="mostrarRegistroSesion"/>
    
    <img alt="Vue logo" class="mt-5 p-3" src="./assets/logo.png" center>
    <component :is="componentMode"></component>
        <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
  </div>

</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import Slider from './components/ListVideos.vue'
import NavBar from './components/NavBar.vue'
import FormJoinSession from './components/FormJoinSession.vue'
import FormRegisterSession from './components/FormRegisterSession.vue'


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
      this.componentMode = 'FormJoinSession';
    },
    mostrarRegistroSesion(){
      this.componentMode = 'FormRegisterSession';
    },
    mostrarVideos(){
      this.componentMode = 'slider';
    }
  },
  components: {
    Slider,
    NavBar,
    FormJoinSession,
    FormRegisterSession
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


