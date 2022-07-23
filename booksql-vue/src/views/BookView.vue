<template>
  <div class="book">
    <ApolloQuery :query="bookQuery" :variables="{ id: $route.params.id }">
        <template v-slot="{ result: { error, data }, isLoading }">
          <div v-if="isLoading" class="loading apollo">Loading...</div>       
          <div v-else-if="error" class="error apollo">An error occurred</div>        
          <div v-else-if="data">
            <div>{{ data.book.title }}</div>
            <div>{{ data.book.author }}</div>
            <img :src="`${data.book.image}`" alt="cover image" />            
          </div>        
          <div v-else class="no-result apollo">No result :( </div>
        </template>
      </ApolloQuery>
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

      const bookQuery = gql`
       query($id: ID!) {
            book(id: $id) {
                id
                title
                author
                image
                    description
                link
                featured
                category{
                    id
                name
                } 
            }
        }`
    

export default {
  name: 'HomeView',
  data() {
    return {
      categoryQuery,
      booksQuery,
      bookQuery,      
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




