<template>
  <Message :msg="msg" v-show="msg" />
  <div>
    <form class="burger__form" method="POST" @submit="createBurger">

      <div class="input__container">
        <label for="nome">Nome do cliente:</label>
        <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
      </div>

      <div class="input__container">
        <label for="pao">Escolha o pão:</label>
        <select name="pao" id="pao" v-model="pao">
          <option value="">Selecione o seu pão</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
        </select>
      </div>

      <div class="input__container">
        <label for="carne">Escolha a carne do seu Burger:</label>
        <select name="carne" id="carne" v-model="carne">
          <option value="">Selecione o tipo de carne</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
        </select>
      </div>

      <div class="input__container">
        <label class="opicionais__checkbox" for="opcionais">Selecione os opcionais:</label>
        <div class="opicionais__checkbox" v-for="opcional in opcionaisdata" :key="opcional.id">
          <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
            <span class="opicionais__checkbox">{{ opcional.tipo }}</span>
        </div>
      </div>

      <div class="input-container">
        <input class="submit-btn" type="submit" value="Criar meu Burger!">
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
    async createBurger(e) {

      e.preventDefault()

      const data = {
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado"
      }

      const dataJson = JSON.stringify(data)    

      const req = await fetch("http://localhost:3000/burgers", {
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
      
    }
  },
  mounted () {
    this.getIngredientes()
  },
  components: {
    Message
  }
}
</script>

<style scoped>
  .burger__form {
    max-width: 400px;
    margin: 0 auto;
  }

  .input__container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }

  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;;
    padding: 5px 10px;
    border-left: 4px solid #f7a901 ;
  }

  input, select {
    padding: 10px 15px;
    border-radius: 2,5%;
    width: 300px;
  }

  .opicionais__checkbox{
    display: flex;
    flex-wrap: wrap;
    flex-basis: auto;
    align-content: flex-start;
  }

  .submit-btn {
    background-color: #0032a0 ;
    color:#f5ebdc;
    font-weight: bold;
    border: 2px solid #f5ebdc ;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }

  .submit-btn:hover {
    background-color: transparent;
    color: #D01C1F ;
  }
</style>