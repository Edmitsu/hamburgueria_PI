<template>
  <div>
    <div class="nav">
      <router-link id="logo-url" to="/">
        <img class="logo" :src="logo" :alt="alt">
      </router-link>
      <router-link class="nav_button" to="/">Fazer Pedido</router-link>
      <router-link class="nav_button" to="/pedidos">Pedidos</router-link>
    </div>
  </div>
</template>

<script>

export default {
  name: "Navbar",
  props: ["logo", "alt"]
}
</script>

<style scoped>
  .nav {
    background-color: #f5ebdc ;
    border-bottom: 4px solid #111;
    padding: 15px 50px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }

  .nav #logo-url {
    margin: auto;
    margin-left: 0;
  }

  .logo {
    width: 40px;
    height: 40px;
  }

  .nav a {
    color: #f5ebdc ;
    text-decoration: none;
    margin: 12px;
    transition: .5s;
  }

  .nav a:hover {
    background-color: transparent;
    color: #D01C1F;
    font-weight: bold;
  }

  .nav_button{
    background-color: #0032a0 ;
    padding: 10px;
    border-radius: 4%;
    border-color: #f5ebdc ;
    flex-direction: row;
  }
</style>