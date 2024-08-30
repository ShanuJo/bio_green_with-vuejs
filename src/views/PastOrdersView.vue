<template>
  <main class="wrapper">
    <h1>Past Orders</h1>
    <div v-if="orders.length === 0">
      <p>No past orders found.</p>
    </div>
    <div v-else>
      <div v-for="(order, index) in orders" :key="index" class="order">
        <h2>Order Date: {{ new Date(order.date).toLocaleDateString() }}</h2>
        <table class="product-table">
          <thead>
            <tr>
              <td><span class="sr-only">Product Image</span></td>
              <td>Product</td>
              <td>Price</td>
              <td>Quantity</td>
              <td>Total</td>
              <td><span class="sr-only">Actions</span></td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in order.items" :key="item.product">
              <td><i class="icofont-spoon-and-fork icofont-3x"></i></td>
              <td>{{item.product}}</td>
              <td>${{ item.price.toFixed(2) }}</td>
              <td>{{item.quantity}}</td>
              <td>${{item.total}}</td>
              <td><button class="btn btn-dark">Add</button></td>
            </tr>

          </tbody>
        </table>
        <p><strong>Total Amount:</strong> ${{ order.totalAmount }}</p>
      </div>
    </div>
  </main>
</template>


<script>
export default {
  data() {
    return {
      orders: []
    };
  },
  created() {
    // Load past orders from localStorage
    const pastOrders = JSON.parse(localStorage.getItem('pastOrders')) || [];
    this.orders = pastOrders;
  }
}
</script>