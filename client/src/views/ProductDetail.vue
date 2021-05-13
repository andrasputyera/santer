<template>
  <div id="page-wrap" v-if="product">
    <div id="img-wrap">
      <img :src="product.imageUrl" />
    </div>
    <div id="product-details">
      <h1>{{ product.name }}</h1>
      <h3 id="price">{{ product.price }}</h3>
      <p>Average rating: {{ product.averageRating }}</p>
      <button 
        id="add-to-cart"
        v-if="!itemIsInCart && !showSuccessMessage" 
        @click="addToCart"
      >Add to Cart
      </button>
      <button 
        id="add-to-cart"
        class="green-button"
        v-if="!itemIsInCart && showSuccessMessage" 
      >Successfully added item to cart!
      </button>
      <button 
        id="add-to-cart"
        class="gray-button"
        v-if="itemIsInCart" 
      >Item is already in the cart.
      </button>
      <h4>Description</h4>
      <p>{{ product.description }}</p>
    </div>
  </div>
  <NotFound v-else />
</template>

<script>
import axios from 'axios';
import NotFound from '../views/NotFound.vue';

export default {
    name: 'ProductDetail',
    components: { 
      NotFound 
    },
    data() {
      return {
        // product: products.find((product) => product.id === this.$route.params.id)
        product: {},
        cartItems: [],
        showSuccessMessage: false,
      }
    },
    // Computed property to determine whether item is already in the cart
    computed: {
      itemIsInCart() {
        return this.cartItems.some(item => item.id === this.product.id);
      }
    },
    methods: {
      async addToCart() {
        await axios.post('/api/users/12345/cart', {
          productId: this.$route.params.id, 
        });
        this.showSuccessMessage = true;
        setTimeout(() => {
          this.$router.push('/products');
        }, 1500)  
      }
    },
    async created() {
      // Condensed way to write const product = result.data - gets data property and reassigns it to product;
      const { data: product } = await axios.get(`/api/products/${this.$route.params.id}`);
      this.product = product;

      const { data: cartItems } = await axios.get('/api/users/12345/cart');
      this.cartItems = cartItems;
    }
}

</script>

<style scoped>
  #page-wrap {
    margin-top: 16px;
    padding: 16px;
    max-width: 600px;
  }

  #img-wrap {
    text-align: center;
  }

  img {
    width: 400px;
  }

  #product-details {
    padding: 16px;
    position: relative;
  }

  #add-to-cart {
    width: 100%;
  }

  #price {
    position: absolute;
    top: 24px;
    right: 16px;
  }

  .green-button {
    background-color: rgb(67, 176, 67);
  }

  .gray-button {
    background-color:rgb(172, 172, 172);
  }
</style>
