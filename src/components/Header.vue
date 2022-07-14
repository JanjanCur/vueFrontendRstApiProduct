<template>
   <nav class="navbar navbar-default">
    <div class="container-fluid">
      <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
        <ul class="nav navbar-nav">
          <li class="active"><router-link to="/">Product List</router-link></li>
          <li><router-link to="/add_products">Add Product</router-link></li>

        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
  export default {
   name: 'Header' 
  }
</script>