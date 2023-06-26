<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Edit Product</h4>
            </div>
            <div class="card-body">

                <ul class="alert alert-warning" v-if="Object.keys(this.errorList).length > 0">
                    <li class="mb-0 ms-3" v-for="(error, index) in this.errorList" :key="index">
                        {{ error[0] }}
                    </li>
                </ul>

                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" v-model="model.product.name" class="from-control">
                </div>
                <div class="mb-3">
                    <label for="">Price</label>
                    <input type="text" v-model="model.product.price" class="from-control">
                </div>
                <div class="mb-3">
                    <label for="">Description</label>
                    <input type="text" v-model="model.product.description" class="from-control">
                </div>
                <div class="mb-3">
                    <button type="button" @click="updateProduct" class="btn btn-primary">
                        Update
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { routeLocationKey } from 'vue-router';
 
export default {
    name: 'productEdit',
    data(){
        return {
            productId: '',
            errorList: '',
            model: {
                product: {
                    name: '',
                    price: '',
                    description: '',
                }
            }
        }
    },
    mounted(){
        //console.log(this.$route.params.id);
        this.productId = this.$route.params.id;
        this.getProductData(this.$route.params.id);
    },
    methods: {

        getProductData(productId){

            axios.get(`http://localhost:3000/products/${productId}`).then(res => {
                console.log(res.data)

                this.model.product = res.data
            })
            .catch(function (error) {
                if(error.response){
                    
                    if(error.response.status == 404){
                        alert(error.response.data.msg);
                    }
                } 
            });

        },

        updateProduct(){

            var mythis = this;
            axios.put(`http://localhost:3000/products/${this.productId}`, this.model.product).then(res => {

                console.log(res.data)
                //alert(res.data.msg);

                
                this.errorList = ''
            })
            .catch(function (error) {
                if(error.response){
                    
                    if(error.response.status == 400){
                        mythis.errorList = error.response.data.error;
                    }

                    if(error.response.status == 404){
                         alert(error.response.data.msg)
                    }

                } else if (error.request) {
                    console.log(error.request);
                } else {
                    console.log('Error', error.msg);
                }
            })
        }

    },
}
</script>