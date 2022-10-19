<template>
  <Message :msg="msg" v-show="msg" />
  <div class="form__background">
    <form class="burger-form" method="POST" @submit="createBurger">
      <div class="input-container">
        <label for="nome">Nome do Cliente:</label>
        <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
      </div>
      <div class="input-container">
        <label for="pao">Escolha o Pão:</label>
        <select name="pao" id="pao" v-model="pao">
          <option value="">Selecione o seu pão</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
        </select>
      </div>
      <div class="input-container">
        <label for="carne">Escolha a carne do seu Burger:</label>
        <select name="carne" id="carne" v-model="carne">
          <option value="">Selecione o tipo de carne</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
        </select>
      </div>
      <div class="input-container">
        <label for="bebida">Escolha a sua Bebida:</label>
        <select name="bebida" id="bebida" v-model="bebida">
          <option value="">Selecione a Bebida</option>
          <option v-for="bebida in bebidas" :key="bebida.id" :value="bebida.tipo">{{ bebida.tipo }}</option>
        </select>
      </div>
      <div class="input-container">
        <label for="porcao">Escolha o Acompanhamento:</label>
        <select name="porcao" id="porcao" v-model="porcao">
          <option value="">Selecione o acompanhamento</option>
          <option v-for="porcao in porcoes" :key="porcao.id" :value="porcao.tipo">{{ porcao.tipo }}</option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais">Selecione os Opcionais:</label>
        <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
          <input class="checkbox-item" type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>
      <div class="input-container">
        <input class="submit-btn" type="submit" value="Monte o seu Burger!">
      </div>
    </form>
  </div>
</template>

<script>
import Message from './Message'
export default {
  name: "BurgerForm",
  data() {
    return {
      paes: null,
      carnes: null,
      opcionaisdata: null,
      nome: null,
      pao: null,
      carne: null,
      opcionais: [],
      bebidas: null,
      porcoes: null,
      status: "Solicitado",
      msg: null
    }
  },
  methods: {
    async getIngredientes() {
      const req = await fetch('http://localhost:3000/ingredientes')
      const data = await req.json()
      this.paes = data.paes
      this.carnes = data.carnes
      this.opcionaisdata = data.opcionais
    },
    async getAcompanhamentos() {
      const req = await fetch('http://localhost:3000/acompanhamentos')
      const data = await req.json()
      this.bebidas = data.bebidas
      this.porcoes = data.porcoes
    },
    async createBurger(e) {
      e.preventDefault()
      const data = {
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        bebida: this.bebida,
        porcao: this.porcao,
        status: "Solicitado"
      }
      const dataJson = JSON.stringify(data)    

      const req = await fetch("http://localhost:3000/pedidos", {
        method: "POST",
        headers: { "Content-Type" : "application/json" },
        body: dataJson
      });
      const res = await req.json()
      console.log(res)
      
      this.msg = "Pedido realizado com sucesso!"
      
      // clear message
      setTimeout(() => this.msg = "", 3000)
      // limpar campos
      this.nome = ""
      this.carne = ""
      this.pao = ""
      this.opcionais = []
      this.bebida = ""
      this.porcao = ""
      
    }
  },
  mounted () {
    this.getIngredientes(),
    this.getAcompanhamentos()
  },
  components: {
    Message
  }
}
</script>

<style scoped>

  .form__background{
    background-color: #ffc765;       
    border-radius: 5px;
    margin: 25px;
    filter: blur(0px);  
  }
  .burger-form {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    max-width: 400px;
    margin: 50px;
    min-width: 30%;
  }
  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }
  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #502314;
    padding: 5px 10px;
    border-left: 4px solid rgba(150,38,38,0.75);
  }
  input, select {
    padding: 5px 10px;
    width: 380px;
    border-radius: 25px;
  }
  #opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;
  }
  #opcionais-title {
    width: 100%;
  }
  .checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
    color: #502314;
  }
  .checkbox-container span,
  .checkbox-container input {
    width: auto;
  }
  .checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
  }
  .submit-btn {
    background-color: #502314 ;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
    border-radius: 20px;
  }
</style>