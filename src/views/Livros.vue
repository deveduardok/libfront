<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6">
        <h2>Livros</h2>
      </div>
      <div class="col-md-6">
        <button v-on:click="deslogar" class="btn btn-danger pull-right">
          Logout
        </button>
        <button
          v-if="role === 'ROLE_ADMIN'"
          v-on:click="redirect"
          class="btn btn-primary pull-right"
        >
          CADASTRO
        </button>
      </div>
    </div>
    <br />
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Id</th>
          <th>Título</th>
          <th>Autor</th>
          <th>Editora</th>
          <th>Ano</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="livro in livros" :key="livro.id">
          <td>{{ livro.id }}</td>
          <td>{{ livro.titulo }}</td>
          <td>{{ livro.autor }}</td>
          <td>{{ livro.editora }}</td>
          <td>{{ livro.ano }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
import { mapState } from "vuex";
import store from "../store";

export default {
  data() {
    return {
      titulo: "",
      livro: "",
      livros: [],
      role: "",
    };
  },
  computed: {
    ...mapState(["usuario"]),
  },
  methods: {
    deslogar() {
      localStorage.clear();
      store.commit("logout");
      window.location.href = "http://localhost:8081/";
      this.atualizar();
    },
    redirect() {
      window.location.href = "http://localhost:8081/cadastro";
    },
    alert() {
      alert("Sem permissão.");
    },
    cadastrar() {
      if (store.state.role == "ROLE_ADMIN") {
        const token = JSON.parse(localStorage.getItem("token"));
        axios
          .post(
            "http://localhost:8080/livro/cadastrar",
            {
              titulo: this.titulo,
              autor: this.autor,
              editora: this.editora,
              ano: this.ano,
            },
            {
              headers: {
                Authorization: `Bearer ${token}`,
              },
            }
          )
          .then((res) => {
            console.log(res);
            this.titulo = "";
            this.autor = "";
            this.editora = "";
            this.ano = "";
            this.atualizar();
          })
          .catch(() => this.alert());
      } else {
        alert("Você não possui a permissão de ADMIN para inserir dados.");
      }
    },
    atualizar() {
      const token = localStorage.getItem("token");
      axios
        .get("http://localhost:8080/livro/listar", {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        })
        .then((res) => {
          this.livros = res.data;
        })
        .catch((error) => {
          this.livros = [];
        });
    },
  },
  created() {
    this.atualizar();
    this.role = localStorage.getItem("role");
  },
};
</script>
