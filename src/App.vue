<template>
 <section class="posts">
  <PostView v-if="postClicado" :idPost="postClicado">
    <button class="button" @click="postClicado = null ">Voltar</button>
  </PostView>
  <ul v-else>
    <div class="post" v-for="post in posts" :key="post.id" @click="postClicado = post.id">
      <div class="titulo">
        <h3>{{ post.id }}</h3> 
      </div>
      <div class="paragrafo">
        <p>{{ post.title }}</p>
      </div> 
    </div>
  </ul>
    <div class="footerCard" v-if="!postClicado">
      <div class="qtdPagina">
      <p>Página: {{ paginaAtual }} / {{ totalPaginas }}</p>
    </div>
    <div class="buttons">
      <button class=button @click="paginaAnterior">Anterior</button>
      <button class="button" @click="proximaPagina">Próximo</button>
    </div>
  </div>
 </section>
</template>

<script>
import { api } from './services'
import PostView from './components/PostView.vue'

export default {
data () {
  return {
    posts: [],
    postClicado: null,
    totalPaginas: null,
    itensPorPagina: 5,
    paginaAtual: 1,
  }
},
components: {
  PostView,
},
methods: {
  fetchPosts() {
    try {
      this.post = false
      api.get("/posts").then((response) => {
      this.posts = response.data;
      this.totalPaginas = Math.ceil(this.posts.length / this.itensPorPagina);
      const inicioIndex = (this.paginaAtual - 1) * this.itensPorPagina;
      const fimIndex = inicioIndex + this.itensPorPagina;
      this.posts = this.posts.slice(inicioIndex, fimIndex); 
    })
    } catch (error) {
      this.posts = []
    }
  },

  proximaPagina () {
    if (this.paginaAtual < this.totalPaginas) {
      this.paginaAtual++;
      this.fetchPosts();
    }
  },
  paginaAnterior () {
    if (this.paginaAtual > 1) {
      this.paginaAtual--;
      this.fetchPosts();
    }
  }
},
created () {
  this.fetchPosts();
}
 
}
</script>

<style>
section, div ,h1, h3, ul, li, p {
  padding: 0;
  margin: 0;
}
body {
  background: #efc8b1;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
ul {
  margin-top: 40px;
}
.post {
  height: 80px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #bb9d8b;
  max-width: 800px;
  margin: auto;
  margin-bottom: 10px;
  cursor: pointer;
}

.post:hover {
  box-shadow: 0 4px 8px #514644;
}

.titulo {
  display: flex;
  background: #514644;
  height: 100%;
  width: 10%;
}

.titulo h3 {
  margin: auto;
  font-size: 2rem;
  color: #efc8b1;
}

.paragrafo {
  display: flex;
  height: 100%;
  width: 100%;
}

.paragrafo p {
  margin: auto;
  color:#514644 ;
  font-size: 1.3rem;
}

.button {
  background: #514644;
  color: #efc8b1;
  border: none;
  width: 100px;
  height: 25px;
  border-radius: 8%;
  margin-top: 15px;
  cursor: pointer;
}

.button:hover {
  box-shadow: 0 4px 8px #514644;
}

.qtdPagina {
  display: flex;
  justify-content: center;
}

.qtdPagina p {
  color: #514644;
}

.buttons {
  display: flex;
  justify-content: center;
}

.buttons .button {
  margin: 10px;
}

.footerCard {
  margin-top: 35px;
}
</style>
