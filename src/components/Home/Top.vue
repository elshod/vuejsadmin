<template>
    <v-app-bar app color="dark" dark>
        <div class="d-flex align-center">
            <v-img alt="Vuetify Logo" class="shrink mr-2" contain
                src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png" transition="scale-transition"
                width="40" />
            <v-img alt="Vuetify Name" class="shrink mt-1 hidden-sm-and-down" contain min-width="100"
                src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png" width="100" />
        </div>
        <v-spacer></v-spacer>
        <v-btn v-for='(link,index) of links' :key='index' :to="link.l" text class="ml-2">
            {{link.t}}
        </v-btn>
    </v-app-bar>
</template>

<script>
    export default {
        data: () => ({
            links: [
                {l:'/', t:'Home'},
                {l:'/category', t:'Category'},
                {l:'/products', t:'Products'},
                {l:'/news', t:'News'},
            ]
        }),
    }
</script>

<style>

</style>