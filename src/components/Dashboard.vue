<template>
  <div class="burger-table" v-if="pedidos">

    <div class="burger-table-rows">
      <div class="burger-table-row" v-for="pedido in pedidos" :key="pedido.id">
        <details>
          <summary class="burger-table-summary">{{ pedido.id }} : {{ pedido.nome }}</summary>
          <div class="burger-summary-container">
            <label class="burger-table-label">Pão</label>
            <div>{{ pedido.pao }}</div>
            <label class="burger-table-label">Carne</label>
            <div>{{ pedido.carne }}</div>
            <label class="burger-table-label">Opcionais</label>
            <div class="burger-table-opcionais">
              <ul v-for="(opcional, index) in pedido.opcionais" :key="index">
                <li>{{ opcional }}</li>
              </ul>
            </div>
            <label class="burger-table-label">Bebida</label>
            <div>{{ pedido.bebida }}</div>
            <label class="burger-table-label">Porção</label>
            <div>{{ pedido.porcao }}</div>

            <label class="burger-table-label">Status</label>
            <div class="actions__container">
              <select name="status" class="status" @change="updateBurger($event, pedido.id)">
                <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="pedido.status == s.tipo">
                  {{ s.tipo }}
                </option>
              </select>

              <button class="delete-btn" @click="deleteBurger(pedido.id)">Cancelar</button>

            </div>
          </div>
        </details>
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

      const dataJson = JSON.stringify({ status: option });

      const req = await fetch(`http://localhost:3000/pedidos/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson
      });

      const res = await req.json()

      console.log(res)

    }
  },
  mounted() {
    this.getPedidos()
  }
}
</script>

<style scoped>
body {
  min-height: 100vh;
}

.burger-table {
  display: flex;
  background-size: cover;
  overflow: auto;
  max-height: 100vh;
  min-height: 30vh;
  width: 50vw;

}

.burger-table-heading,
.burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}

.burger-table-heading {
  padding: 1em;
  background-color: #502314;
  border-radius: 10px;
  color: white;
  opacity: 0.6;
  border-bottom: 3px solid #333;
}

.burger-table-label {
  display: none;
}

.burger-table-row {
  width: 100%;
  padding: 25px;
  border-bottom: 1px solid #CCC;
}

.burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 10%;
}

.burger-table-opcionais {
  padding: 15px;
}

.burger-summary-container{
  padding: 10px;
  font-size: 20px;
}

.burger-table-summary{
  font-size: 20px;
}

.order-number {
  padding: 0.5em;
  height: 1em;
  align-items: center;
  font-weight: 900;
  font-size: 20px;
}

.actions__container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.status {
  padding: 10px;
  border-radius: 5px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  max-width: 150px;
}

.delete-btn {
  background-color: #502314;
  color: #f5ebdc;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: .5s;
  border-radius: 5px;
  min-width: 150px;
}

.delete-btn:hover {
  color: #fcba03;
}

@media only screen and (max-width: 600px) {
  .burger-table-heading {
    display: none;
  }

  burger-table-rows,
  div {
    display: flex;
    flex-direction: column;
    background-color: #ffc765;
    border-radius: 1.5em;
    align-items: center;
    gap: 1.5rem;
  }

  .burger-table-row {
    display: flex;
    flex-direction: column;
    flex-wrap: nowrap;
  }

  .burger-table-label {
    display: flex;
    justify-content: center;
    font-size: 2em;
    font-weight: bolder;
    color: #f5ebdc;
  }


  ul {
    list-style-type: none;
  }

  .burger-table-summary {
    align-items: center;
  }

  .burger-table {
    display: flex;
    background-size: cover;
    justify-content: flex-start;
    min-height: 75vh;
    max-height: 75vh;
    width: 75vw;
  }

  details {
    justify-content: center;
  }

  details[open] {
    align-items: center;
    justify-content: center;
  }
}
</style>