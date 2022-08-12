<template>
  <div id="app" class="container">
    <div id="nav" class="row text-center">
        <router-link to="/">Login </router-link> |
        <router-link to="/home">Home</router-link> | 
      <router-link to="/livros">Livros</router-link>
    </div>
    <router-view/>
</div>  
</template>

