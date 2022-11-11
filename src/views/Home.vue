<template>
  <div class="main__banner">
    <BurgerForm class="form"/>    
  </div>
</template>

<script>
import Banner from '../components/Banner'
import BurgerForm from '../components/BurgerForm'


export default {
  name: 'Home',
  components: {
    Banner,
    BurgerForm,
    
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

</style>