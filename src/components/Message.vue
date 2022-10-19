<template>
  <div class="message-container">
    <p>{{ msg }}</p>
  </div>
</template>

<script>
  export default {
    name: "Message",
    props: {
      msg: String
    }
  }
</script>

<style scoped>
  .message-container {
    color: #f7a901;
    background-color: #0032a0;
    border: 2px solid #0032a0;
    border-radius: 5px;
    padding: 10px;
    max-width: 400px;
    margin: 30px auto;
    text-align: center;
  }
</style>