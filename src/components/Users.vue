<template lang="html">
  <div>
    <div v-if="$apollo.loading">
      <div class="loader"></div>
      <p>Buscando dados...</p>
    </div>
    <div v-if="user.id">
      <hr>
      <h2>USUÁRIO</h2>
      <p>ID: {{ user.id }}</p>
      <p>NOME: {{ user.name }}</p>
      <p>GRUPO: {{ user.group.descricao }}</p>
    </div>
    <div v-if="user.posts.length">
      <h4>POSTS</h4>
      <ul>
        <li v-for="post in user.posts">
          {{ post.title }} - {{ post.body}}
        </li>
      </ul>
    </div>
    <div v-if="groups.length">
      <hr>
      <h2>GRUPOS</h2>
      <ul>
        <li v-for="(grupos, key) in groups" :key="key">{{ grupos.descricao }}</li>
      </ul>
    </div>
    <button @click="makeRequest()">Carregar informações</button>
    <button @click="incrementId()" v-if="!lazyLoading">Buscar proximo usuário</button>
  </div>
</template>

<script>
import gql from 'graphql-tag';
const userQuery = gql`query($id: Int) {
  user(id: $id) {
    id
    name
    group {
      descricao
    }
    posts {
      title
      body
    }
  }
  groups {
    descricao
  }
}`

export default {
  data: () => ({
    id: 1,
    user: {
      id: null,
      name: null,
      group: {},
      posts: [],
    },
    groups: [],
    lazyLoading: true,
  }),
  apollo: {
    data: {
      query: userQuery,
      variables() {
        return { id: this.id }
      },
      skip() {
        return this.lazyLoading
      },
      update({ user, groups }) {
        this.user = user
        this.groups = groups
      },
    },
  },
  methods: {
    makeRequest() {
      this.lazyLoading = false;
    },
    incrementId() {
      this.id++;
    }
  },
};
</script>

<style lang="css">
.loader {
  margin: 0 auto;
  border: 16px solid #f3f3f3;
  border-top: 16px solid #3498db;
  border-radius: 50%;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
