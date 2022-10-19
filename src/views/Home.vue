<template>
  <div>
    <Banner />
  </div>
</template>

<script>
import Banner from '../components/Banner'
import BurgerForm from '../components/BurgerForm'
import Combos from '../components/Combos'

export default {
  name: 'Home',
  components: {
    Banner,
    BurgerForm,
    Combos
  }
}
</script>