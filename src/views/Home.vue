<template>
  <v-container>
    <v-card>
      <v-list-item v-for="cat of category" :key="cat.id">
        <v-list-item-content>
          <v-list-item-title >
            <router-link :to="'/category/'+cat.id">
              {{cat.title}}

            </router-link>
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios'

  export default {
    data: () => ({
      category: []
    }),
    created(){
      axios.get('http://localhost:3000/category').then(response => {this.category = response.data})
    }
  }
</script>
