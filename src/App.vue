<template>
  <div id="app" class="mt-5 p-5">
    
    <NavBar
      v-on:abrirInicioSesion="mostrarInicioSesion" 
      v-on:mostrarVideos="mostrarVideos"
      v-on:abrirRegistroSesion="mostrarRegistroSesion"
      v-on:subirVideo="subirVideo"
      v-on:verVideo="verVideo"
      v-on:cerrarMiSesion="cerrarSesion"/>
    
    <component :is="componentMode"
      v-on:sessionJoined="sessionJoined"
      v-on:abrirInicioSesion="mostrarInicioSesion">
    </component>
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
      idSession: null,
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
    },
    sessionJoined(id){
      this.componentMode = 'slider';
      localStorage.setItem('yutulSessionId' , id);
    },
    cerrarSesion(){
      localStorage.setItem('yutulSessionId' , null);
      this.idSession = null;
    }
  },
  components: {
    Slider,
    NavBar,
    FormJoinSession,
    FormRegisterSession,
    FormUploadVideo,
    FormViewVideo
  },
  created(){
    alert('Bienvenido user : ' +  localStorage.getItem('yutulSessionId') );
    this.idSession = localStorage.getItem('yutulSessionId');
  }
}
</script>

<style>
/*
*/
</style>


