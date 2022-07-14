<template>
  
      <div class="container">
          <div class="row">
              <div class="col-md-6">
              <div class="alert alert-danger mt-4" v-if="errors.length">
                <ul class="mb-0">
                    <li v-for="(error, index) in errors" :key="index">
                        {{error}}
                    </li>
                </ul>  
              </div >
                  <form @submit.prevent="saveProduct" novalidate>
                    <fieldset>
                      <div class="form-group">
                        <label class="form-label mt-4">Name</label>
                        <input type="text" class="form-control" placeholder="Enter Name" v-model="product.name">
                      </div>
                      <div class="form-group">
                        <label class="form-label mt-4">Description</label>
                        <input type="text" class="form-control" placeholder="Enter Description" v-model="product.description">
                      </div>
                      <div class="form-group">
                        <label class="form-label mt-4">Price</label>
                        <input type="text" class="form-control" placeholder="Enter Price"  v-model="product.price">
                      </div>
                      <div class="form-group">
                        <label class="form-label mt-4">Quantity</label>
                        <input type="text" class="form-control" placeholder="Enter Quantity" v-model="product.quantity">
                      </div>
                      <div class="form-group">
                        <label class="form-label mt-4">Category</label>
                        <select class="form-control" v-model="product.category_id">
                          <option  v-for="category in categories" :key="category.id" :value="category.id">{{category.name}}</option>
                        </select>
                      </div>
                      <button class="btn btn-primary mt-4">Submit</button>
                    </fieldset>
                  </form>
              </div>
          </div>
      </div>

</template>

<script>
  import axios from 'axios';
  export default {
    name: 'AddProduct',
    data(){
        return {
            product: {},
            name: '',
            description: '',
            price: '',
            quantity: '',
            category_id: '',
            errors: [],
            categories:Array,
        }

    },
   created() {
    this.getCategories();
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
       async saveProduct() {
            this.errors = [];
            if(!this.product.name){
                this.errors.push("Name is Required"); 
            }
            if(!this.product.description){
                this.errors.push("Description is Required"); 
            }
            if(!this.product.price){
                this.errors.push("Price is Required"); 
            }
            if(!this.product.quantity){
                this.errors.push("Quantity is Required"); 
            }
            if(!this.product.category_id){
                this.errors.push("Category is Required"); 
            }
            if(!this.errors.length){
                let formData = new FormData();
                formData.append('name', this.product.name);
                formData.append('description', this.product.description);
                formData.append('price', this.product.price);
                formData.append('quantity', this.product.quantity);
                formData.append('category_id', this.product.category_id);
                let url = 'http://localhost:8000/api/save-product';
               await  axios.post(url, formData).then((response) => {
                  console.log(this.response);
                  if(response.status = 200){
                  this.product.name = '';
                  this.product.description = '';
                  this.product.price = '';
                  this.product.quantity = '';
                  this.product.category_id = '';
                  alert(response.data.message);
                 } else {
                     console.log('error');
                 }  
                }).catch(error => {
                    this.errors.push(error.response);
                });

            }
        } 
    }
  }
</script>