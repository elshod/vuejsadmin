<template>
  <v-app>
    <Top/>
    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>
import Top from './components/Home/Top'
export default {
  name: 'App',
  components: {
    Top,
  }
  
};
</script>
