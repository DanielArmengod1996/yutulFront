<template>    
<div>
  <b-navbar class="fixed-top" type="dark" variant="dark">
    <b-navbar-nav>
      <b-nav-item v-on:click="mostrarVideos">Home</b-nav-item>


      <b-nav-item-dropdown text="Sesion" left>
        <b-dropdown-item v-on:click="iniciarSesion">Join Session</b-dropdown-item> 
        <b-dropdown-item v-on:click="abrirRegistroSesion">Register</b-dropdown-item> 

      </b-nav-item-dropdown>
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
    }
  }
}
</script>
