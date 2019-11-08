<template>
  <div id="app" class="mt-5 p-5">
    
    <NavBar  
      v-on:abrirInicioSesion="mostrarInicioSesion" 
      v-on:mostrarVideos="mostrarVideos"
      v-on:abrirRegistroSesion="mostrarRegistroSesion"
      v-on:subirVideo="subirVideo"
      v-on:verVideo="verVideo"/>
    
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
import FormUploadVideo from './components/FormUploadVideo.vue'
import FormViewVideo from './components/FormViewVideo.vue'


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
    },
    subirVideo(){
      this.componentMode = 'FormUploadVideo';
    },
    verVideo(){
      this.componentMode = 'FormViewVideo';
    }
  },
  components: {
    Slider,
    NavBar,
    FormJoinSession,
    FormRegisterSession,
    FormUploadVideo,
    FormViewVideo
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


