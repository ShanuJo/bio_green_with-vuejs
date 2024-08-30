<template>
  <aside class="cart-container">
    <div class="cart">
      <h1 class="cart-title spread">
        <span>
          Cart
          <i class="icofont-cart-alt icofont-1x"></i>
        </span>
        <button @click="toggle" class="cart-close">&times;</button>
      </h1>

      <div class="cart-body">
        <table class="cart-table">
          <thead>
            <tr v-if="Object.keys(cart).length">
              <th><span class="sr-only">Image</span></th>
              <th>Product</th>
              <th>Price</th>
              <th>Qty</th>
              <th>Total</th>
              <th><span class="sr-only">Actions</span></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(quantity, key, i) in cart" :key="i">
              <td><i class="icofont-spoon-and-fork icofont-3x"></i></td>
              <td>{{ key }}</td>
              <td>${{ getPrice(key) }}</td>
              <td class="center">{{ quantity }}</td>
              <td>${{ (quantity * getPrice(key)).toFixed(2) }}</td>
              <td class="center">
                <button @click="remove(key)" class="btn btn-light cart-remove">
                  &times;
                </button>
              </td>
            </tr>
          </tbody>
        </table>

        <p class="center" v-if="!Object.keys(cart).length"><em>No items in cart</em></p>
        <div class="spread">
          <span><strong>Total:</strong> ${{ calculateTotal() }}</span>
          <button class="btn btn-light" @click="handleCheckout">Checkout</button>
        </div>
      </div>
    </div>
  </aside>
</template>

<script>
export default {
  props: ['toggle', 'cart', 'inventory', 'remove', 'saveCart'],
  methods: {
    getPrice(name) {
      const product = this.inventory.find((item) => item.name === name);
      return product ? product.price.USD : 0;
    },
    calculateTotal() {
      const total = Object.entries(this.cart).reduce((sum, [key, quantity]) => {
        return sum + (quantity * this.getPrice(key));
      }, 0);
      return total.toFixed(2);
    },
    handleCheckout() {
      const order = {
        items: Object.entries(this.cart).map(([key, quantity]) => ({
          product: key,
          price: this.getPrice(key),
          quantity,
          total: (quantity * this.getPrice(key)).toFixed(2),
        })),
        totalAmount: this.calculateTotal(),
        date: new Date().toISOString(),
      };

      // Save order to pastOrders
      const pastOrders = JSON.parse(localStorage.getItem('pastOrders')) || [];
      pastOrders.push(order);
      localStorage.setItem('pastOrders', JSON.stringify(pastOrders));

      // Clear the cart
      this.$emit('clear-cart');
      this.toggle(); // Optionally close the cart
    }
  }
}
</script>
