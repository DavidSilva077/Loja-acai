<template>
  <Message :msg="msg" v-show="msg" />
  <div>
    <form id="acai-form" method="POST" @submit="createacai">
      <div class="input-container">
        <label for="nome">Nome do cliente:</label>
        <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
      </div>
      <div class="input-container">
        <label for="sorvete">Escolha o Sorvete:</label>
        <select name="sorvete" id="sorvete" v-model="sorvete">
          <option value="">Selecione o seu sorvete</option>
          <option v-for="sorvete in sorvetes" :key="sorvete.id" :value="sorvete.tipo">{{ sorvete.tipo }}</option>
        </select>
      </div>
      <div class="input-container">
        <label for="cobertura">Escolha a cobertura do seu açaí:</label>
        <select name="cobertura" id="cobertura" v-model="cobertura">
          <option value="">Selecione o tipo de cobertura</option>
          <option v-for="cobertura in coberturas" :key="cobertura.id" :value="cobertura.tipo">{{ cobertura.tipo }}</option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
        <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
          <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>
      <div class="input-container">
        <input class="submit-btn" type="submit" value="Criar meu açaí!">
      </div>
    </form>
  </div>
</template>

<script>
import Message from './Message'

export default {
  name: "acaiForm",
  data() {
    return {
      sorvetes: null,
      coberturas: null,
      opcionaisdata: null,
      nome: null,
      sorvete: null,
      cobertura: null,
      opcionais: [],
      status: "Solicitado",
      msg: null
    }
  },
  methods: {
    async getIngredientes() {
      const req = await fetch('http://localhost:3000/ingredientes')
      const data = await req.json()

      this.sorvetes = data.sorvetes
      this.coberturas = data.coberturas
      this.opcionaisdata = data.opcionais
    },
    async createacai(e) {

      e.preventDefault()

      const data = {
        nome: this.nome,
        cobertura: this.cobertura,
        sorvete: this.sorvete,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado"
      }

      const dataJson = JSON.stringify(data)    

      const req = await fetch("http://localhost:3000/acais", {
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
      this.cobertura = ""
      this.sorvete = ""
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
  #acai-form {
    max-width: 400px;
    margin: 0 auto;
  }

  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }

  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
  }

  input, select {
    padding: 5px 10px;
    width: 300px;
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

  .submit-btn:hover {
    background-color: transparent;
    color: #222;
  }
</style>