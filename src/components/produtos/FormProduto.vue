<template>
  <div class="">
    <h2>{{ produto_editar.produto === "" ? "cadastrar" : "editar" }}</h2>

    <div class="form-group">
      <label for="nome">Produto</label>
      <input
        type="text"
        class="form-control"
        id="Nome do produto"
        placeholder="Enter email"
        v-model="produto.nome"
      />
    </div>
    <div class="form-group">
      <label for="preco">Preco</label>
      <input
        type="number"
        class="form-control"
        id="preco"
        placeholder="Preço"
        v-model="produto.preco"
      />
    </div>

    <button @click="save" class="btn btn-primary mt-2">Submit</button>
  </div>
</template>

<script>
import axios from "axios";
import { reactive } from "vue";
import { useRoute } from "vue-router";

export default {
  setup() {
    const route = useRoute();
    const produto_editar = route.params;

    console.log(produto_editar);
    const produto = reactive({
      id: null,
      nome: "",
      preco: "",
    });

    if (produto_editar.produto !== "") {
      (produto.id = produto_editar.id),
        (produto.nome = produto_editar.nome),
        (produto.preco = produto_editar.preco);
    }

    return {
      produto,
      produto_editar,
    };
  },
  methods: {
    async save() {
      console.log(this.produto);

      if (this.produto.id === null) {
        await axios
          .post("http://localhost:3001/produtos", {
            nome: this.produto.nome,
            preco: this.produto.preco,
          })
          .then(function (response) {
            console.log("redirecionar...");
          })
          .catch(function (error) {
            console.log(error);
          });
      } else {
        await axios
          .patch(`http://localhost:3001/produtos/${this.produto.id}`, {
            nome: this.produto.nome,
            preco: this.produto.preco,
          })
          .then(function (response) {
            console.log("redirecionar...");
          })
          .catch(function (error) {
            console.log(error);
          });
      }
      this.$router.push({
        name: "produtos",
      });
    },
  },
};
</script>
