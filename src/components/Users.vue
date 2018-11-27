<template lang="html">
  <div>
    <p v-if="loading">Carregando...</p>
    <ul>
      <li v-for="(user,key) in users.data" :key="key">
        {{ user.id }} -
        {{ user.name }} |
        {{ user.group.descricao }}
      </li>
    </ul>
    <button @click="carregar = !carregar">Carregar informações</button>
    <button @click="id++">Add ID</button>
  </div>
</template>

<script>
import gql from 'graphql-tag';
const userQuery = gql`query($id: Int) {
  users(id: $id) {
    data {
      id
      name
      group {
        descricao
      }
    }
    per_page
    total
    from
    to
    current_page
  }
}`

export default {
  data: () => ({
    id: 1,
    users: {
      data: [],
    },
    loading: false,
    carregar: true,
  }),
  apollo: {
    users: {
      query: userQuery,
      loadingKey: 'loading',
      variables() {
        return { id: this.id }
      },
      skip() {
        return this.carregar
      },
    },
    // groups: gql`query {
    //   groups {
    //     id
    //   }
    // }`
  },
  methods: {
    getData() {
      console.log(this.$apollo);
      console.log(this.$apollo.query(
        {query: userQuery}
      ));
      this.$apollo.query(
        {query: userQuery, loadingKey: this.loading}
      ).then(response => console.log(response))
      // this.$apollo.queries.users.destroy();
      // this.$apollo.queries.users.skip = false;
      // this.$apollo.queries.users.refetch();
    },
  },
  computed: {
    teste() {
      console.log(this.$apolloData.data);
    },
  },
};
</script>
