<template>
    <v-container class="mt-3">
        <v-row>
            <h2>Yangiliklar ro'yhati</h2>
            <v-spacer></v-spacer>
            <v-btn color="success" @click="dialog = !dialog">
                Yangi maqola
            </v-btn>


            <v-dialog v-model="dialog" max-width="600px" transition="dialog-bottom-transition">
                <v-card>
                    <v-card-title>Yangi maqola</v-card-title>
                    <v-card-text>
                        <v-container>
                            <v-row>
                                <v-col cols="12">
                                    <v-text-field label="Maqola sarlavhasi*" v-model="post.title" required>
                                    </v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field label="Maqola linki" v-model="post.link"
                                        hint="link kichkina harf va probelsiz bo`lishi lozim. probel o`rniga chiziq (-) ishlating">
                                    </v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field label="Maqola rasmi" v-model="post.img">
                                    </v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-select
                                        :items="['Olimov Hamid', 'Ergashev Aziz', 'Tohirov Farhod', 'Polvonova Aziza']"
                                        v-model="post.author" label="Maqola muallifini tanlang*" required>
                                    </v-select>
                                </v-col>
                                <v-col cols="12">
                                    <vue-editor v-model="post.content" />
                                </v-col>
                            </v-row>
                        </v-container>
                    </v-card-text>
                    <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="blue darken-1" text @click="dialog = false">
                            Yopish
                        </v-btn>
                        <v-btn color="blue darken-1" text @click="dialog = false, add()">
                            Joylashtirish
                        </v-btn>
                    </v-card-actions>
                </v-card>
            </v-dialog>
        </v-row>
        <v-row>
            <v-col cols='4' v-for="post of posts" :key='post.id'>
                <v-card class="mx-auto">
                    <v-img class="white--text align-end" height="200px" :src="post.img">
                        <v-card-title>{{post.title}}</v-card-title>
                    </v-img>

                    <v-card-subtitle class="pb-0">
                        {{post.author}}
                    </v-card-subtitle>
                    <v-card-actions>
                        <v-btn color="orange" text @click="openpost = post, opendialog = true"> 
                            Batafsil
                        </v-btn>
                    </v-card-actions>
                </v-card>
            </v-col>
        </v-row>
        <v-dialog v-model="opendialog" width="750px">
            <v-card>
                <v-card-title>
                    <span class="headline">{{openpost.title}}</span>
                    <span class="subtitle-2">{{openpost.author}}</span>
                </v-card-title>
                <v-img class="white--text align-end mb-4" height="400px" :src="openpost.img">
                </v-img>
                <v-card-text v-html="openpost.content"></v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="green darken-1" text @click="opendialog = false">
                        Yopish
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-container>
</template>

<script>
    import {
        VueEditor
    } from "vue2-editor"
    import axios from 'axios'
    export default {
        components: {
            VueEditor
        },
        data: () => ({
            dialog:false, 
            opendialog: false,
            post: {},
            openpost: {},
            posts: []
        }),
        methods: {
            add() {
                axios.post('http://localhost:3000/post', this.post).then(response => {
                    this.posts.push(response.data)
                })
                this.post = {}
            }
        },
        created() {
            axios.get('http://localhost:3000/post').then(response => {
                this.posts = response.data
            })
        }
    }
</script>

<style>

</style>