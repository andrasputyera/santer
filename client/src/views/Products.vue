<template>
  <div id="page-wrap">
    <ProductsGrid :products="products" />
  </div>
</template>

<script>
import axios from 'axios';
import ProductsGrid from '../components/ProductsGrid.vue';

export default {
  name: 'Products',
  components: {
    ProductsGrid
  },
  data() {
    return {
      products: [],
    }
  },
  async created() {
    const result = await axios.get('/api/products');
    const products = result.data;
    // Set the products array to the products recieved from the server
    this.products = products;
  }
}
</script>
