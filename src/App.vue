<script setup>
import { RouterLink, RouterView } from 'vue-router';
import Sidebar from '@/components/SidebarComponent.vue';
import food from './food.json';
</script>
<script>
export default {
  components: {
    Sidebar
  },
  data() {
    return {
      showSidebar: false,
      inventory: food,
      cart: {}
    };
  },
  computed: {
    totalQuantity() {
      return Object.values(this.cart).reduce((acc, curr) => {
        return acc + curr;
      }, 0);
    }
  },
  methods: {
    addToCart(name, quantity) {
      if (!this.cart[name]) {
        this.cart[name] = 0;
        console.log(this.cart[name]);
      }
      this.cart[name] += quantity;
      console.log('cart:', this.cart);
    },
    toggleSidebar() {
      this.showSidebar = !this.showSidebar;
    },
    removeItem(name) {
      delete this.cart[name];
    }
  }
};
</script>

<template>
  <header class="top-bar spread">
    <nav class="top-bar-nav">
      <RouterLink to="/" class="top-bar-link">
        <i class="icofont-spoon-and-fork"></i>
        <span>Home</span>
      </RouterLink>
      <RouterLink to="/products" class="top-bar-link">
        <span>Products</span>
      </RouterLink>
      <RouterLink to="/past-orders" class="top-bar-link">
        <span>Past Orders</span>
      </RouterLink>
    </nav>
    <div @click="toggleSidebar" class="top-bar-cart-link">
      <i class="icofont-cart-alt icofont-1x"></i>
      <span>Cart ({{ totalQuantity }})</span>
    </div>
  </header>

  <RouterView :inventory="inventory" :addToCart="addToCart" />
  <Sidebar
    :cart="cart"
    v-if="showSidebar"
    :toggle="toggleSidebar"
    :inventory="inventory"
    :remove="removeItem"
  />
</template>
