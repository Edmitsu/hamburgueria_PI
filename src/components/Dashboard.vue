<template>
  <div class="burger-table" v-if="pedidos">
    <div>
      <div class="burger-table-heading">
        <div class="order-id">Numero do Pedido:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Bebida:</div>
        <div>Acompanhamento:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div class="burger-table-rows">
      <div class="burger-table-row" v-for="pedido in pedidos" :key="pedido.id">
        <div class="order-number">{{ pedido.id }}</div>
        <div>{{ pedido.nome }}</div>
        <div>{{ pedido.pao }}</div>
        <div>{{ pedido.carne }}</div>
        <div>
          <ul v-for="(opcional, index) in pedido.opcionais" :key="index">
            <li>{{ opcional }}</li>
          </ul>
        </div>
        <div>{{ pedido.bebida }}</div>
        <div>{{ pedido.porcao }}</div>
        

        <div>
          <select name="status" class="status" @change="updateBurger($event, pedido.id)">
            <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="pedido.status == s.tipo">
              {{ s.tipo }}
            </option>
          </select>

          <button class="delete-btn" @click="deleteBurger(pedido.id)">Cancelar</button>

        </div>
      </div>
    </div>
  </div>

  <div v-else>
    <h2>Não há pedidos no momento!</h2>
  </div>
</template>
<script>
  export default {
    name: "Dashboard",
    data() {
      return {
        pedidos: null,
        pedido_id: null,
        status: []
      }
    },
    methods: {
      async getPedidos() {
        const req = await fetch('http://localhost:3000/pedidos')

        const data = await req.json()

        this.pedidos = data

        // Resgata os status de pedidos
        this.getStatus()

      },
      async getStatus() {

        const req = await fetch('http://localhost:3000/status')

        const data = await req.json()

        this.status = data

      },
      async deleteBurger(id) {

        const req = await fetch(`http://localhost:3000/pedidos/${id}`, {
          method: "DELETE"
        });

        const res = await req.json()

        this.getPedidos()

      },
      async updateBurger(event, id) {

        const option = event.target.value;

        const dataJson = JSON.stringify({status: option});

        const req = await fetch(`http://localhost:3000/pedidos/${id}`, {
          method: "PATCH",
          headers: { "Content-Type" : "application/json" },
          body: dataJson
        });

        const res = await req.json()

        console.log(res)

      }
    },
    mounted () {
    this.getPedidos()
    }
  }
</script>

<style scoped>
  .burger-table {
    max-width: 1200px;
    margin: 0 auto;
  }

  .burger-table-heading,
  .burger-table-rows,
  .burger-table-row {
    display: flex;
    flex-wrap: wrap;
  }

  .burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }

  .burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }

  .burger-table-heading div,
  .burger-table-row div {
    width: 10%;
    margin: auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
  }

  .burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 10%;
  }

  .status {
    padding: 12px 6px;
    margin-right: 12px;
  }

  .delete-btn {
    background-color: #0032a0;
    color:#f5ebdc;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  
  .delete-btn:hover {
    background-color: transparent;
    color: #D01C1F;
  }
  
</style>