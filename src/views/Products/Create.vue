<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Add Product</h4>
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
                    <button type="button" @click="saveProduct" class="btn btn-primary">
                        Save
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
 
export default {
    name: 'productCreate',
    data(){
        return {
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
    methods: {

        saveProduct(){

            var mythis = this;
            axios.post('http://localhost:3000/products', this.model.product).then(res => {

                console.log(res.data)
                //alert(res.data.msg);

                this.model.product = {
                    name: '',
                    price: '',
                    description: '',
                }
                this.errorList = ''
            })
            .catch(function (error) {
                if(error.response){
                    
                    if(error.response.status == 400){
                        mythis.errorList = error.response.data.error;
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