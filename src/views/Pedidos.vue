<template>
  <div class="main__banner">
    <div class="main-container">
      <h1>Gerenciar Pedidos</h1>
      <Dashboard />
    </div>
  </div>
</template>

<script>
import Dashboard from '../components/Dashboard'


export default {
  name: 'Pedidos',
  components: {
    Dashboard,
  }
}
</script>

<style scoped>
.main__banner {
  display: flex;
  flex-direction: column;
  align-items: flex-start;      
  background-image: url('/home/edmitsu/Hamburgueria_PI/public/img/burger.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  height: 800px;
}

.main-container{
  background-color: #ffc765;
  padding: 12px;
  margin: 0 auto;
  border-radius: 10px;
  font-weight: 600;
  box-shadow: #502314  6px 6px 5px;

}

h1{
  padding: 12px;
  background-color: #502314;
  border-radius: 10px;
  color: white;
  width: fit-content;
  opacity: 0.8;
}
</style>