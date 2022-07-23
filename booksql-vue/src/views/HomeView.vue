<template>
  <div class="home">
    <router-link to="/books/add">Add a Book</router-link>
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
    >
      <template v-slot="{ result: { error, data }, isLoading }">
        <!-- Loading -->
        <div v-if="isLoading" class="loading apollo">Loading...</div>

        <!-- Error -->
        <div v-else-if="error" class="error apollo">An error occurred</div>

        <!-- Result -->
        <div v-else-if="data" class="category" >
            <a href="#" class="link-margin" @click="selectCategory('all')">All</a>
            <!-- <a href="#" class="link-margin" @click="selectCategory('featured')">Featured</a> -->
            <a href="#" v-for="category in data.categories" 
              :key="category.id" @click.prevent="selectCategory(category.id)" class="link-margin">
              {{ category.id }}. {{ category.name }}
            </a>
          
        </div>
        <!-- No result -->
        <div v-else class="no-result apollo">No result :( </div>
      </template>
    </ApolloQuery>

    <div v-if="selectedCategory === 'all'">
      <ApolloQuery :query="booksQuery">
        <template v-slot="{ result: { error, data }, isLoading }">
          <div v-if="isLoading" class="loading apollo">Loading...</div>       
          <div v-else-if="error" class="error apollo">An error occurred</div>        
          <div v-else-if="data">
            <div v-for="book in data.books" :key="book.id">
              <router-link :to="`/books/${book.id}`">
                {{ book.id }}. {{ book.title }}
              </router-link>
              <div>{{ book.author }}</div>
              <img :src="`${book.image}`" alt="cover image" />            
            </div>
          </div>        
          <div v-else class="no-result apollo">No result :( </div>
        </template>
      </ApolloQuery>
    </div>

    <div v-else-if="selectedCategory === 'featured'">
      <ApolloQuery :query="booksFeaturedQuery" :variables="{featured: true}">
        <template v-slot="{ result: { error, data }, isLoading }">
          <div v-if="isLoading" class="loading apollo">Loading...</div>       
          <div v-else-if="error" class="error apollo">An error occurred</div>        
          <div v-else-if="data">
            <div v-for="book in data.booksByFeatured" :key="book.id">
              <router-link :to="`/books/${book.id}`">
                {{ book.id }}. {{ book.title }}
              </router-link>
              <div>{{ book.author }}</div>
              <img :src="`${book.image}`" alt="cover image" />  
            </div>
          </div>        
          <div v-else class="no-result apollo">No result :( </div>
        </template>
      </ApolloQuery>
    </div>
    
    <div v-else>
      <ApolloQuery :query="query" :variables="{ id: selectedCategory }">
        <template v-slot="{ result: { error, data }, isLoading }">
          <div v-if="isLoading" class="loading apollo">Loading...</div>
          <div v-else-if="error" class="error apollo">An error occurred</div>
          <div v-else-if="data.category">
            <div v-for="book in data.category.books" :key="book.id">
              <router-link :to="`/books/${book.id}`">
                {{ book.id }}. {{ book.title }}
              </router-link>
              <div>{{ book.author }}</div>
              <img :src="`${book.image}`" alt="cover image" />  
            </div>
          </div>
          <div v-else class="no-result apollo">No result :( </div>
        </template>
      </ApolloQuery>
    </div>
    

  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import gql from 'graphql-tag'
// import categoryQuery from '@/graphql/queries/Category.gql'
// import booksQuery from '@/graphql/queries/Books.gql'
// import categoryQuery from '@/graphql/queries/Category.gql'

const categoryQuery = gql`
        query ($id: ID!) {
          category (id: $id) {
            id
            name
            books {
              id
              title
              author
              image
            }
          }
      }`       

  const booksQuery = gql`
        query {
          books {
            id
            title
            author
            image
          }
      }`

    // const booksFeaturedQuery = gql`
    //   query {
    //     booksByFeatured(featured: true) {
    //       id
    //       title
    //       author
    //       image
    //     }
    //   }`

export default {
  name: 'HomeView',
  data() {
    return {
      categoryQuery,
      booksQuery,
      // booksFeaturedQuery,      
      query: categoryQuery,
      selectedCategory: 'all',
      categories: []
    }
  },   

  methods: {
    selectCategory(category) {
      if (category === 'all') {
        this.query = booksQuery
        this.selectedCategory = 'all'
      // } else if (category === 'featured') {
      //   this.query = booksFeaturedQuery
      //   this.selectedCategory = 'featured'
      } else {
        this.query = categoryQuery
        this.selectedCategory = category
      }      
        // this.selectedCategory = category
    }
  },
  
  components: {
    // HelloWorld
  }
}
</script>

<style scoped>
.link-margin {
  margin-right: 24px;
}
</style>
