<template>
    <v-container class="mt-4">
        <v-row>
            <h2>Mahsulotlar ro'yhati</h2>
            <v-spacer></v-spacer>
            <v-btn color="primary" @click="dialog = !dialog">
                <v-icon>
                    mdi-cart-plus
                </v-icon>
                <span class="ml-2">Yangi mahsulot</span>
            </v-btn>

        </v-row>
        <v-row>
            <v-col cols="12">
                <v-data-table :headers="headers" :items="products" :items-per-page="10" class="elevation-1">
                    <template v-slot:item.btns="{ item }">
                        <div class="text-right">
                            <v-btn color="info" @click="show(item)">
                                <v-icon>
                                    mdi-eye
                                </v-icon>
                            </v-btn>
                            <v-btn class="ml-3" color="warning" @click="edit(item)">
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
        <v-dialog  max-width="600px" v-model="isShow">

                <v-card>
                    <v-toolbar color="primary" dark>Mahsulot ma'lumotlari</v-toolbar>
                    <v-card-text>
                        <div class="text-h2 pa-12">{{showProduct.title}}</div>
                        <div class="text-h4 pa-12">{{showProduct.link}}</div>
                        <div v-html='showProduct.content'>
                            
                        </div>
                    </v-card-text>
                    <v-card-actions class="justify-end">
                        <v-btn text @click="isShow = false">Yopish</v-btn>
                    </v-card-actions>
                </v-card>
        </v-dialog>
        <v-dialog v-model="dialog" max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline" v-show="!toggle">Yangi mahsulot</span>
                    <span class="headline" v-show="toggle">Mahsulotni tahrirlash</span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-row>
                            <v-col cols="12">
                                <v-text-field label="Mahsulot nomi*" v-model='product.title' required></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field label="Mahsulot link*" v-model='link' required></v-text-field>
                            </v-col>
                            <v-col cols="12" sm="6">
                                <v-autocomplete :items="categories" item-text="title" item-value="id"
                                    label="Qaysi kategoriyaga tegishli" multiple></v-autocomplete>
                            </v-col>
                            <v-col cols="12" sm="6">
                                <v-autocomplete v-model="product.comp" :items="company" label="Ishlab chiqaruvchi">
                                </v-autocomplete>
                            </v-col>
                            <v-col cols='6'>
                                <v-text-field label="Mahsulot narhi" v-model='product.price' required></v-text-field>

                            </v-col>
                            <v-col cols='6'>
                                <v-slider v-model="product.price" min="0" max="15000000" label="Narhi">
                                </v-slider>
                            </v-col>
                            <v-col cols="12">
                                <vue-editor v-model="product.content"></vue-editor>
                            </v-col>
                        </v-row>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="dialog = false, product={}">
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
    import {
        VueEditor
    } from "vue2-editor";
    import axios from 'axios'
    export default {
        components: {
            VueEditor
        },
        computed: {
            link() {
                return this.product.title.split(' ').join('-').toLowerCase().replaceAll(',', '').replaceAll('/', '')
            }
        },
        data: () => ({
            isShow:false,
            showProduct:{},
            dialog: false,
            toggle: false,
            categories: [],
            product: {
                title: '',
                link: ''
            },
            products: [],
            company: ['Xiaomi', 'Samsung', 'Apple', 'Vivo', 'LG', 'Huawei'],
            headers: [{
                    text: 'Sarlavha',
                    value: 'title'
                },
                {
                    text: 'Havola',
                    value: 'link'
                },
                {
                    text: 'Mavjudligi',
                    value: 'hide'
                },
                {
                    text: '',
                    value: 'btns'
                },
            ],
        }),
        methods: {
            show(item){
                this.showProduct = item 
                this.isShow = true
            },
            save() {
                this.dialog = false
                this.product.link = this.link
                this.toggle = false
                axios.put('http://localhost:3000/product/' + this.product.id, this.product)
                    .then(response => {
                        this.products.forEach(cat => {
                            if (cat.id == response.data.id) {
                                cat = response.data
                            }
                        })
                    })
                this.product = {
                    title: '',
                    link: ''
                }
                this.link = ''
            },
            edit(item) {
                this.product = item
                this.toggle = true
                this.dialog = true
            },
            del(id) {

                axios.delete('http://localhost:3000/product/' + id).then(response => {
                    console.log(response)
                    this.products = this.products.filter(cat => {
                        return cat.id !== id
                    })

                })
            },
            add() {
                this.product.link = this.link
                this.link = ''
                axios.post('http://localhost:3000/product', this.product)
                    .then(response => {
                        this.products.push(response.data)
                    })
                this.product = {
                    title: '',
                    link: ''
                }
                this.dialog = false
            }
        },
        created() {
            axios.get('http://localhost:3000/product')
                .then(response => {
                    this.products = response.data
                })
            axios.get('http://localhost:3000/category')
                .then(response => {
                    this.categories = response.data
                })
        }
    }
</script>

<style>

</style>