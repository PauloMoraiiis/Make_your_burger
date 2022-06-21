<template>
  <div class="main-container">
    <h1>Gerenciar pedidos:</h1>
    <Dashboard /> <!-- Carrega componente dashboard -->
  </div>
</template>

<script>
// Diretorios de componentes importados
import Dashboard from '../components/Dashboard.vue'
export default {
  // Nome deste componente
  name: "Pedidos",
  // Componentes usados
  components: {
    Dashboard
  }
}
</script>
