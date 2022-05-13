<template>
  <div id="acai-table" v-if="acais">
    <div>
      <div id="acai-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Soverte:</div>
        <div>cobertura:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="acai-table-rows">
      <div class="acai-table-row" v-for="acai in acais" :key="acai.id">
        <div class="order-number">{{ acai.id }}</div>
        <div>{{ acai.nome }}</div>
        <div>{{ acai.sorvete }}</div>
        <div>{{ acai.cobertura }}</div>
        <div>
          <ul v-for="(opcional, index) in acai.opcionais" :key="index">
            <li>{{ opcional }}</li>
          </ul>
        </div>
        <div>
          <select name="status" class="status" @change="updateacai($event, acai.id)">
            <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="acai.status == s.tipo">
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteacai(acai.id)">Cancelar</button>
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
        acais: null,
        acai_id: null,
        status: []
      }
    },
    methods: {
      async getPedidos() {
        const req = await fetch('http://localhost:3000/acais')

        const data = await req.json()

        this.acais = data

        // Resgata os status de pedidos
        this.getStatus()

      },
      async getStatus() {

        const req = await fetch('http://localhost:3000/status')

        const data = await req.json()

        this.status = data

      },
      async deleteacai(id) {

        const req = await fetch(`http://localhost:3000/acais/${id}`, {
          method: "DELETE"
        });

        const res = await req.json()

        this.getPedidos()

      },
      async updateacai(event, id) {

        const option = event.target.value;

        const dataJson = JSON.stringify({status: option});

        const req = await fetch(`http://localhost:3000/acais/${id}`, {
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
  #acai-table {
    max-width: 1200px;
    margin: 0 auto;
  }

  #acai-table-heading,
  #acai-table-rows,
  .acai-table-row {
    display: flex;
    flex-wrap: wrap;
  }

  #acai-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }

  .acai-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }

  #acai-table-heading div,
  .acai-table-row div {
    width: 19%;
  }

  #acai-table-heading .order-id,
  .acai-table-row .order-number {
    width: 5%;
  }

  select {
    padding: 12px 6px;
    margin-right: 12px;
  }

  .delete-btn {
    background-color: #222;
    color:#fcba03;
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
    color: #222;
  }
  
</style>