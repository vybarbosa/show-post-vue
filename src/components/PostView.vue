<template>
    <section>
      <div class="page">
        <div class="tituloPage">
          <h1>{{ postClicado.title }}</h1>
        </div>
        <div class="paragrafoPage">
          <p>{{ postClicado.body }}</p>
        </div>
        <slot></slot>
      </div>
    </section>
</template>
<script>
import { api } from '@/services';

export default {
  name: 'PostView',
  props: ['idPost'],
  data() {
    return {
      postClicado:[]
    }
  },
  methods: {
    fetchPost(){
      api.get(`/posts/${this.idPost}`).then((response) => {
        this.postClicado = response.data;
      })

    }
  },
  created () {
    this.fetchPost();
  }
}
</script>

<style scoped>
.page {
  max-width: 800px;
  margin: auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin-top: 50px;
}

.tituloPage {
  width: 100%;
  height: 80px;
  background: #514644;
  display: flex;
  align-items: center;
  justify-content: center;
}

.tituloPage h1 {
  font-size: 1.3rem;
  color: #efc8b1;
}

.paragrafoPage {
  width: 100%;
  height: 400px;
  background: #bb9d8b;
}

.paragrafoPage p {
 font-size: 1.1rem;
 margin-left: 10px;
}


</style>