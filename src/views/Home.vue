<template>
<div>
  <Banner /> <!-- Carreca componente Banner -->
  <div class="main-container">
    <h1>Monte o seu Burger:</h1>
    <BurgerForm /> <!-- Carreca componente BurgerForm -->
  </div>
</div>
</template>

<script>
// Diretorios de componentes importados
import Banner from '../components/Banner.vue'
import BurgerForm from '../components/BurgerForm.vue'

export default {
    // Nome deste componente
    name: "Home",
    // Componentes usados
    components: {
        Banner,
        BurgerForm
    }
}

</script>
