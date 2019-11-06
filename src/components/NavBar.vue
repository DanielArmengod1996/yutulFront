<template>    
<div>
  <b-navbar class="fixed-top" type="dark" variant="dark">
    <b-navbar-nav>
      <b-nav-item v-on:click="mostrarVideos">Home</b-nav-item>


      <b-nav-item-dropdown text="Sesion" left>
        <b-dropdown-item v-on:click="iniciarSesion">Join Session</b-dropdown-item> 
        <b-dropdown-item v-on:click="abrirRegistroSesion">Register</b-dropdown-item> 

      </b-nav-item-dropdown>
      <b-nav-item v-on:click="subirVideo">Subir Video</b-nav-item>
      <b-nav-item v-on:click="verVideo">Ver Video</b-nav-item>

    </b-navbar-nav>
  </b-navbar>
  
</div>
</template>
<script>

export default {
  components: {
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
    }
  }
}
</script>
