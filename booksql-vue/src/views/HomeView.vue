<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <div v-for="category in categories" :key="category.id">
      {{category.namr}}
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import gql from 'graphql-tag'

export default {
  name: 'HomeView',
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
