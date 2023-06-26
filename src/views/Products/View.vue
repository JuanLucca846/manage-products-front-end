
<template>
<div>
    <div class="card">
        <div class="card-header">
            <h4>
                Products
                <RouterLink to="/products/create" class="btn btn-primary float-end">
                    Add Product
                </RouterLink>
            </h4>
        </div>
        <div class="card-body">
            <table class="table table-bordered">
                <thead>
                    <tr>
                        <th>Id</th>
                        <th>Name</th>
                        <th>Price</th>
                        <th>Description</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(products, index) in this.products.rows" :key="index">
                        <td>{{ products.id }}</td>
                        <td>{{ products.name }}</td>
                        <td>{{ products.price }}</td>
                        <td>{{ products.description }}</td>
                        <td>
                            <RouterLink :to="{path: '/products/'+products.id+'/edit'}" class="btn btn-primary float=end">
                                Edit
                            </RouterLink>
                            <button type="button" @click="deleteProduct(products.id)" class="btn btn-danger">
                                Delete
                            </button>
                        </td>
                    </tr>
                </tbody>             
            </table>
        </div>
    </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'products',
    data(){
        return {
            products: []
        }
    },
    mounted(){

        this.getProducts();
       //console.log("teste")

    },
    methods: {
        getProducts(){

            axios.get('http://localhost:3000/products').then(res => {
                this.products.rows = res.data.products.rows
                //console.log(this.products)
            });
        },
        deleteProduct(productId){
            console.log(productId)
            if(confirm('Are you sure, you want do delete this product?')){
                console.log(productId)
                axios.delete(`http://localhost:3000/products/${productId}`).then(res => {
                    alert(res.data.msg);
                    this.getProducts();
                })
                .catch(function (error) {
                if(error.response){
                    
                    if(error.response.status == 404){
                        alert(error.response.data.msg);
                    }
                } 
            });
            }
        },
    },
}

</script>