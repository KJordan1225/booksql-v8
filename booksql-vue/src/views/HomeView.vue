<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> --> 
    <ApolloQuery
      :query="gql => gql`
        query {
        categories {
          id
          name
        }
      }
      `"
      :variables="{ id, name }"
    >
      <template v-slot="{ result: { loading, error, data } }">
        <!-- Loading -->
        <div v-if="loading" class="loading apollo">Loading...</div>

        <!-- Error -->
        <div v-else-if="error" class="error apollo">An error occurred</div>

        <!-- Result -->
        <div v-else-if="data" class="category" >
          <ul>
            <li v-for="category in categories" :key="category.id" class="category">
              {{ category.id }} {{ category.name }}
            </li>
          </ul>
        </div>
        <!-- No result -->
        <div v-else class="no-result apollo">No result :(</div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import gql from 'graphql-tag'

export default {
  name: 'HomeView',
  data() {
    return {
      categories: []
    }
  },
  apollo: {
    categories: gql`query {
      categories {
        id
        name
      }
    }`,
  },
  components: {
    // HelloWorld
  }
}
</script>
