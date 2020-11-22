<template>    
<div>
  <b-navbar class="fixed-top" type="dark" variant="dark">
    <b-navbar-nav>
      <b-nav-item v-on:click="mostrarVideos">Home</b-nav-item>


      <b-nav-item-dropdown  text="Sesion" left>
        <b-dropdown-item  v-if="sessionId == null" v-on:click="iniciarSesion">Join Session</b-dropdown-item> 
        <b-dropdown-item  v-on:click="verVideo">Mis videos</b-dropdown-item> 
        <b-dropdown-item  v-if="sessionId == null" v-on:click="abrirRegistroSesion">Register</b-dropdown-item> 
        <b-dropdown-item  v-if="sessionId != null" v-on:click="cerrarMiSesion">Close Session</b-dropdown-item> 
      </b-nav-item-dropdown>
      <b-nav-item v-if="sessionId != null" v-on:click="subirVideo">Subir Video</b-nav-item>
    </b-navbar-nav>
  </b-navbar>
  
</div>
</template>
<script>

export default {
    data() {
        return {
            sessionId : ( this.userId == 'null' ) ? null : this.userId
        }
    },
    name: 'NavBar',
    props: {
        userId: String
    },
    methods:{
        iniciarSesion(){
        this.$emit('abrirInicioSesion');
        },
        abrirRegistroSesion(){
        this.$emit('abrirRegistroSesion');
        },
        mostrarVideos(){
        this.$emit('mostrarVideos');
        },
        subirVideo(){
        this.$emit('subirVideo');
        },
        verVideo(){
        this.$emit('verVideo');
        },
        cerrarMiSesion(){
        this.$emit('cerrarMiSesion');
        }
    }
}
</script>
