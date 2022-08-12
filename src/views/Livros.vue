
<template>
<div class="container">
<div class="row">
        <div class="col-md-6">

        <h2>Livros</h2>
        </div>
          <div class="col-md-6">

        <button v-on:click="deslogar" class="btn btn-danger pull-right">Logout</button>
        </div>
    </div>
    <form @submit.prevent="cadastrar">
      <div class="form-group">
        <label for="titulo">Título</label>
        <input type="text" id="titulo"
            class="form-control" required autofocus
            v-model="titulo">
      </div>
      <div class="form-group">
        <label for="autor">Autor</label>
        <textarea id="autor"
            class="form-control" required
            v-model="autor">
        </textarea>
      </div>
      <div class="form-group">
        <label for="editora">Editora</label>
        <textarea id="editora"
            class="form-control" required
            v-model="editora">
        </textarea>
      </div>
      <div class="form-group">
        <label for="ano">Ano</label>
        <textarea id="ano"
            class="form-control" required
            v-model="ano">
        </textarea>
      </div>
      <button class="btn btn-lg btn-primary btn-block" 
        type="submit">Salvar</button>
    </form>
    <br>
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
import axios from 'axios'
import { mapState } from 'vuex'
import store from '../store'

export default {
  data() {
    return {
      titulo: '',
      livro: '',
      livros: []
    }
  },
  computed: {
    ...mapState([
      'usuario'
    ])
  },
  methods: {
  deslogar(){
    store.commit('logout')
    this.atualizar()
  },
    alert(){
      alert("Sem permissão.")
    },
    cadastrar() {
      if(store.state.role == 'ROLE_ADMIN'){
      const token = JSON.parse(localStorage.getItem("token"))
      axios.post('http://localhost:8080/livro/cadastrar',
          {
            titulo: this.titulo,
            autor: this.autor,
            editora: this.editora,
            ano: this.ano,
          }, {
            headers: {
              Authorization: `Bearer ${token}`
            }
          })
        .then(res => {
          console.log(res)
          this.titulo = '';
          this.autor = '';
          this.editora = '';
          this.ano = '';
          this.atualizar();
        })
        .catch(() => this.alert()  
          )      
        }else{
          alert("Você não possui a permissão de ADMIN para inserir dados.")
        }
      
    },
    atualizar () {
            const token = JSON.parse(localStorage.getItem("token"))

      axios.get('http://localhost:8080/livro/listar', 
          {  headers: {
              Authorization: `Bearer ${token}`
            } })
        .then(res => {
          console.log(res)
          this.livros = res.data
        })
        .catch(error => { 
          this.livros=[]
          console.log(error) 
          } )
    }
  },
  created () {
    this.atualizar()
  },
}
</script>