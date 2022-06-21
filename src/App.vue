<template>
  <Navbar :logo="logo_src" :alt="app_name" /> <!-- Carrega componente Navbar -->
  <router-view/>
  <Footer /> <!-- Carrega componente Footer -->
</template>

<script>
// Diretorios de componentes importados
import Navbar from './components/Navbar.vue'
import Footer from './components/Footer.vue'

export default {
  components: {
    // Componentes usados
    Navbar,
    Footer
  },
  data() {
    return {
      logo_src: "/img/logo.png",
      app_name: "Make Your Burguer"
    }
  }
}
</script>

<style>
 * {
  font-family: helvetica;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
 }

 .main-container {
  margin: 50px;
  min-height: 250px;
 }

 h1 {
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
 }

</style>
