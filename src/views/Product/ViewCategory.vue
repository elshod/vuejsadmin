<template>
    <v-container>
        <h1 class="mb-4">Категория: {{category.title}}</h1>
        <v-row>
            <v-col cols='4' v-for="product of products" :key='product.id'>
                <v-card>
                    <v-card-text>
                        <div>{{product.price}} so'm</div>
                        <p class="display-1 text--primary">
                            {{product.title}}
                        </p>
                        <p>{{product.company}}</p>
                        <div class="text--primary">
                            relating to or dependent on charity; charitable.<br>
                            "an eleemosynary educational institution."
                        </div>
                    </v-card-text>
                    <v-card-actions>
                        <v-btn text color="teal accent-4" @click="opendialog = true, pro = product">
                            Batafsil
                        </v-btn>
                    </v-card-actions>
                </v-card>
            </v-col>
        </v-row>
        <v-dialog v-model="opendialog" width="750px">
            <v-card>
                <v-card-title>
                    <div class="headline">
                        {{pro.title}}
                        <div class="subtitle-1">{{pro.comp}}</div>
                        <div class="header">{{pro.price}} so'm</div>

                    </div>

                    

                    
                </v-card-title>
                <v-card-text v-html="pro.content"></v-card-text>
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
    import axios from 'axios'
    export default {
        data() {
            return {
                id: this.$route.params.id,
                products: [],
                category: {},
                opendialog: false,
                pro: {}
            }
        },
        created() {
            axios.get('http://localhost:3000/product?category=' + this.id).then(response => {
                this.products = response.data
            })
            axios.get('http://localhost:3000/category/'+this.id).then(response => {this.category = response.data})
        }
    }
</script>

<style>

</style>