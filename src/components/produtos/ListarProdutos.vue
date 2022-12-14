<script>
import axios from "axios";

export default {
  data() {
    return {
      produtos: [],
      produto: {},
    };
  },
  created() {
    this.getProdutos();
  },
  methods: {
    async getProdutos() {
      try {
        this.produtos = await (
          await await axios.get("http://localhost:3001/produtos")
        ).data;
        console.log(produtos);
      } catch (err) {
        console.log(err);
      }
    },

    async removeProduto(id) {
      let x = window.confirm("Tem certeza que desaja remover esse produto?");

      if (x) {
        axios.delete(`http://localhost:3001/produtos/${id}`);
        this.getProdutos();
      }
    },

    async editarProduto(produto) {
      this.produto = produto;
      console.log(produto);
      this.$router.push({
        path: "/form-produtos",
        name: "form-produto",
        params: produto,
      });
    },
  },
};
</script>
<template>
  <div>
    <div class="row d-flex justify-content-center">
      <router-link class="btn btn-primary" to="/form-produto"
        >Novo produto
      </router-link>
    </div>

    <table class="table">
      <thead>
        <tr>
          <th scope="col">Código</th>
          <th scope="col">Nome</th>
          <th scope="col">Preço</th>
          <th scope="col">Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="produto in produtos" :key="produto.id">
          <th scope="row">{{ produto.id }}</th>
          <td>{{ produto.nome }}</td>
          <td>{{ produto.preco }}</td>
          <td class="btn btn-warning p-2" @click="editarProduto(produto)">
            Editar
          </td>
          <td class="btn btn-danger p-2" @click="removeProduto(produto.id)">
            Deletar
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
