<template>

<header class="top-bar spread">
        <nav class="top-bar-nav">
          <router-link to="/" class="top-bar-link">
            <i class="icofont-spoon-and-fork"></i>
            <span>Home</span>
          </router-link>
          <router-link to="/products" class="top-bar-link">
            <span>Products</span>
          </router-link>
          <router-link to="/past-orders" class="top-bar-link">
            <span>Past Orders</span>
          </router-link>
        </nav>

        <div @click="toggleSidebar" class="top-bar-cart-link">
          <i class="icofont-cart-alt icofont-1x"></i>
          <span>Cart ({{totalQuantity}})</span>
        </div>
      </header>

  <router-view :inventory="inventory" :addToCart="addToCart" />

  <Sidebar
      :cart="cart"
      :inventory="inventory"
      v-if="showSidebar"
      :toggle="toggleSidebar"
      :remove="removeItem"
      :saveCart="saveCartToLocalStorage"
    @clear-cart="clearCart"
  />
</template>

<script>
import food from './food.json'
import Sidebar from '@/components/Sidebar.vue'

export default {
  components: {
    Sidebar
  },
  data () {
    return {
      showSidebar: false,
      inventory: food,
      cart: {}
    }
  },
  created() {
    this.cart = this.loadCartFromLocalStorage();
  },
  computed: {
      totalQuantity () {
        return Object.values(this.cart).reduce((sum, curr) => {
          return sum + curr
        }, 0)
      }
    },
    methods: {
      addToCart(name, quantity) {
        if(!this.cart[name]) this.cart[name] = 0;
        this.cart[name] += quantity
        // this.inventory[index].quantity = 0
        this.saveCartToLocalStorage(this.cart);
      },
      toggleSidebar(){
        this.showSidebar = !this.showSidebar;
      },
      removeItem(name){
        delete this.cart[name];
        this.saveCartToLocalStorage(this.cart);
      },
      saveCartToLocalStorage(cart) {
      localStorage.setItem('cart', JSON.stringify(cart));
    },
    loadCartFromLocalStorage() {
      try {
        const cart = localStorage.getItem('cart');
        return cart ? JSON.parse(cart) : {};
      } catch (e) {
        console.error('Error loading cart from localStorage:', e);
        return {};
      }
    },
    clearCart() {
      this.cart = {}; // Clear the cart in the parent component
      this.saveCartToLocalStorage(this.cart); // Update local storage
    }
    },
    
}
</script>
