<template>
    <v-container class="mt-4">
        <v-row>
            <h2>Kategoriyalar ro'yhati</h2>
            <v-spacer></v-spacer>
            <v-btn color="primary" @click="dialog = !dialog">
                <v-icon>
                    mdi-cart-plus
                </v-icon>
                <span class="ml-2">Yangi kategoriya</span>
            </v-btn>
            
        </v-row>
        <v-row>
            <v-col cols="12">
                <v-data-table
                    :headers="headers"
                    :items="categories"
                    :items-per-page="10"
                    class="elevation-1"
                >
                    <template v-slot:item.btns="{ item }">
                        <div class="text-right">
                            <v-btn color="warning" @click="edit(item)">
                            <v-icon>
                                mdi-pencil
                            </v-icon>
                            </v-btn>
                            <v-btn class="ml-3" color="error" @click="del(item.id)">
                                <v-icon>mdi-cart-remove</v-icon>
                            </v-btn>
                        </div>
                    </template>
                </v-data-table>
            </v-col>
        </v-row>
        <v-dialog v-model="dialog" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline" v-show="!toggle">Yangi kategoriya</span>
                    <span class="headline" v-show="toggle">Kategoriyani tahrirlash</span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-row>
                            <v-col cols="6">
                                <v-text-field label="Kategoriya nomi*" v-model='category.title' required></v-text-field>
                            </v-col>
                            <v-col cols="6">
                                <v-text-field label="Kategoriya link*" v-model='category.link' required></v-text-field>
                            </v-col>
                            <v-col cols="12" sm="12">
                                <v-checkbox 
                                v-model="category.hide"
                                label="Vaqtincha mavjud emas:"></v-checkbox>
                            </v-col>
                        </v-row>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="dialog = false, category={}">
                        Yopish
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="add()" v-show="!toggle">
                        Qo'shish
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="save()" v-show="toggle">
                        Saqlash
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-container>

</template>

<script>
import axios from 'axios'
    export default {
        data: () => ({
            dialog:false,
            toggle:false,
            category:{},
            categories: [],
            headers: [
                { text: 'Sarlavha', value: 'title' },
                { text: 'Havola', value: 'link' },
                { text: 'Mavjudligi', value: 'hide' },
                { text: '', value: 'btns' },
            ],
        }),
        methods:{
            save(){
                this.dialog = false 
                this.category = {}
                this.toggle = false
                axios.put('http://localhost:3000/category/'+this.category.id,this.category)
                    .then(response => { 
                        this.categories.forEach(cat => {
                            if (cat.id == response.data.id) {
                                cat = response.data
                            }
                        }) 
                    })
            },
            edit(item){
                this.category = item
                this.toggle   = true
                this.dialog   = true
            },
            del(id){
                
                axios.delete('http://localhost:3000/category/'+id).then(response=> {
                    console.log(response)
                    this.categories = this.categories.filter(cat => {
                        return cat.id !== id
                    })

                })
            },
            add(){
                axios.post('http://localhost:3000/category',this.category)
                    .then(response => { this.categories.push(response.data) })
                this.category = {}
                this.dialog = false
            }
        },
        created(){
            axios.get('http://localhost:3000/category')
                    .then(response => { this.categories = response.data })
        }
    }
</script>

<style>

</style>