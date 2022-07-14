<template>
    <div>
      <form @submit.prevent="filterProducts">
        <fieldset>
          <div class="form-group">
            <label class="form-label mt-4">Filter</label>
            <select class="form-control" v-model="category_id">
              <option value="all">All</option>
              <option value="no-stocks">No Stocks</option>
              <option value="with-stocks">With Stocks</option>
            </select>
          </div>
          <button class="btn btn-primary mt-4">Filter</button>
        </fieldset>
      </form>
    <table class="table table-hover">
            <thead>
                <tr>
                    <th scope="col">Name</th>
                    <th scope="col">Description</th>
                    <th scope="col">Price</th>
                    <th scope="col">Quantity</th>
                    <th scope="col">Category</th>
                    <th scope="col">Action</th>
                </tr>
            </thead>
            <tbody v-for="product in products" :key="product.id"> 
                <tr class="table-secondary">
                    <th scope="row">{{product.name}}</th>
                    <th scope="row">{{product.description}}</th>
                    <th scope="row">{{product.price}}</th>
                    <th scope="row">{{product.quantity}}</th>
                    <th scope="row">{{product.category.name}}</th>
                    <th scope="row"><button class="btn btn-danger btn-sm" @click.prevent="deleteProduct(product.id)">Delete</button></th>
                    <th scope="row"><router-link :to="{ name: 'EditProduct', params: { id: product.id }}" class="btn btn-primary btn-sm" >Edit</router-link></th>

                </tr>
            </tbody>
    </table>

    </div>
</template>

<script>
import axios from 'axios';
  export default {
   name: 'ProductList',
   data() {
       return {
        products:Array,
        category_id: '',
        categories:Array,

       }
   },
   created() {
    this.getProducts();
//    this.filterProducts();
   },
    methods: {
    
        async getCategories(){
            let url = 'http://localhost:8000/api/categories';
            await axios.get(url).then(response => {
                this.categories = response.data.categories;
                console.log(this.categories);
            }).catch(error => {
                console.log(error);
            });
        },
        async getProducts(){
            let url = 'http://localhost:8000/api/products';
            await axios.get(url).then(response => {
                this.products = response.data.products;
                console.log(this.products);
            }).catch(error => {
                console.log(error);
            });
        },
        async filterProducts(){
            let url = `http://localhost:8000/api/filter-products?filter_type=${this.category_id}`;
            await axios.get(url).then(response => {
                this.products = response.data.products;
                console.log(this.products);
                
            }).catch(error => {
                console.log(error);
            });
        },
        async deleteProduct(id){
            let url = `http://localhost:8000/api/delete-product/${id}`;
            await axios.delete(url).then(response => {
                alert(response.data.message);
                this.getProducts();
            }).catch(error => {
                console.log(error);
            });
        }
    },
    mounted(){
        console.log('Contact List Component');
    }
   }
</script>