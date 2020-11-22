<template>
  <div id="app" class="mt-5 p-5">
    
    <NavBar
      v-on:abrirInicioSesion="mostrarInicioSesion" 
      v-on:mostrarVideos="mostrarVideos"
      v-on:abrirRegistroSesion="mostrarRegistroSesion"
      v-on:subirVideo="subirVideo"
      v-on:verVideo="verVideo"
      v-on:cerrarMiSesion="cerrarSesion"
      :userId="usuarioId"/>
    
    <component :is="componentMode"
      v-on:sessionJoined="sessionJoined"
      v-on:abrirInicioSesion="mostrarInicioSesion"
      :userId="usuarioId">
    </component>
  </div>

</template>

<script>
import Slider from './components/ListVideos.vue'
import NavBar from './components/NavBar.vue'
import FormJoinSession from './components/FormJoinSession.vue'
import FormRegisterSession from './components/FormRegisterSession.vue'
import FormUploadVideo from './components/FormUploadVideo.vue'
import FormViewVideo from './components/FormViewVideo.vue'


export default {
    components: {
        Slider,
        NavBar,
        FormJoinSession,
        FormRegisterSession,
        FormUploadVideo,
        FormViewVideo
    },
    name: 'app',
    data() {
        return {
            componentMode: 'Slider',
            usuarioId : localStorage.getItem('yutulSessionId')
        }
    },
    methods:{
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
        sessionJoined(){
            location.reload();
        },
        cerrarSesion(){
            localStorage.setItem('yutulSessionId' , null);
            location.reload();
        }
    },
    beforeCreate() {
    }
}
</script>

<style>
/*
*/
</style>


