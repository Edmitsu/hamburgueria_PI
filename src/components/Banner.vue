<template>
  <div class="main__banner">
    <h1 class="main__text">Hamburgueria PI</h1>
  </div>
</template>

<script>
  export default {
    name: "Banner"
  }
</script>

<style scoped>

  .main__banner {
    background-image: url('/img/burger.jpg');
    background-position: center;
    background-size: cover;
    height: 500px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
  }
  .main__text{
    max-width: 75%;
    color: #f5ebdc ;
    text-align: center;
    background-color: #D01C1F ;
    padding: 20px 40px;
    border-radius: 5%;
  }

</style>