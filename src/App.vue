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
  <PostsPaginar :postsTotal="postsTotal" :postsPorPagina="postsPorPagina" />
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
  }
},
components: {
  PostView,
},
methods: {
  fetchPosts() {
    this.post = false
    api.get("/posts").then((response) => {
      this.posts = response.data;
    })
  },
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

</style>
