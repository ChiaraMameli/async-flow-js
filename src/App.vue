<template>
  <div class="container">
    <h1>Search for a post ID</h1>
    <input v-model="postId" @keyup.enter="fetchData" type="number">
    <button @click="fetchData"> search </button>

    <!-- Viene stampato solo se il valore di post è diverso da null(dato di partenza) -->
    <article v-if="post != null">
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </article>

    <!-- Viene stampato solo se il valore di user è diverso da null(dato di partenza) -->
    <article v-if="user != null">
      <p>This post was written by: </p>
      <h3>{{ user.name }} - {{ user.email }}</h3>
      <address>
        {{ user.address.city }}, {{ user.address.city }} ({{ user.address.zipcode }})
      </address>
    </article>

  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'App',
  data(){
    return {
      uri: "https://jsonplaceholder.typicode.com/",
      post: null,
      postId: 1,
      user: null,
      userId: 1,
    }
  },
  methods:{
    //metodo che esegue chiamata API, restituisce post
    fetchPost(){
      axios.get(`${this.uri}posts/${this.postId}`).then((res) => {
        this.post = res.data;
        this.userId = res.data.userId;
      }).catch(err => {
        console.log(err);
      })
    },
    //metodo che esegue chiamata API, restituisce autore del post
    fetchUser(){
      axios.get(`${this.uri}users/${this.userId}`).then((res) => {
              this.user = res.data
          }).catch(err => {
            console.log(err);
          })
    },
    /* Questo metodo viene chiamato al click del bottone e al press di enter nell'input. 
    * Recupera dall'input l'id del post, verifica che il dato sia accettabile (che sia un numero è controllato 
    * anche dall'HTML), e richiama i metodi di cui sopra.
    */ 
    fetchData(){
      if(this.postId <= 0 || this.postId > 100 || isNaN(this.postId)) alert("Insert value is not valid");
      else {
        this.fetchPost();
        if(this.userId != null) this.fetchUser();
      }
    }
  }
}
</script>

<style lang="scss">
.container {
  margin: 0 auto;
  padding-top: 2rem;
  box-sizing: border-box;
  width: 80%;

  button {
    margin-left: 0.2rem;
  }

  article {
    border: 0.1rem solid #000;
    margin: 1rem 2rem;
    padding: 1rem;
  }
}
</style>
