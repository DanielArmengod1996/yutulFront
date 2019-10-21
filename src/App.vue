<template>
  <div id="app">
    <NavBar/>
    <img alt="Vue logo" src="./assets/logo.png">
    <Form class="formulario"/>
    <Slider id="videoSlider"/>
    <img alt="Vue logo" src="./assets/logo.png">


    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->

  </div>

</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import Slider from './components/Slider.vue'
import NavBar from './components/NavBar.vue'
import Form from './components/Form.vue'



export default {
  name: 'app',
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


