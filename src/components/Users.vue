<template lang="html">
  <div>
    <pre>{{ users }}</pre>
    <pre>{{ teste }}</pre>
    <pre>{{ groups }}</pre>
  </div>
</template>

<script>
import gql from 'graphql-tag';
export default {
  apollo: {
    users: gql`query {
      users(id: 3) {
        data {
          id
          name
        }
        per_page
        total
        from
        to
        current_page
      }
    }`,
    groups: gql`query {
      groups {
        id
      }
    }`
  },
  computed: {
    teste() {
      console.log(this.$apolloData.data);
    },
  },
};
</script>
