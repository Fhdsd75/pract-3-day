<template>
    <div class="order-history">
      <h2>История заказов</h2>
      <div v-if="orders.length">
        <div
          v-for="(order, index) in orders"
          :key="index"
          class="order"
        >
          <p><strong>Дата заказа:</strong> {{ order.date }}</p>
          <p><strong>Ф.И.О.:</strong> {{ order.form.name }}</p>
          <p><strong>Телефон:</strong> {{ order.form.phone }}</p>
          <p><strong>Адрес:</strong> {{ order.form.address }}</p>
          <p><strong>Товары:</strong></p>
          <ul>
            <li v-for="item in order.cartItems" :key="item.id">
              <img :src="item.image" alt="Фото товара" class="item-image" />
              {{ item.name }} - {{ item.quantity }} шт. ({{ item.price * item.quantity }} KZT)
            </li>
          </ul>
          <p><strong>Общая стоимость:</strong> {{ order.totalCost }} KZT</p>
        </div>
      </div>
      <p v-else>История заказов пуста.</p>
    </div>
  </template>
  
  <script>
  export default {
    name: "OrderHistory",
    data() {
      return {
        orders: [], // Пустой массив для загрузки заказов
      };
    },
    mounted() {
      // Загружаем заказы из localStorage при монтировании компонента
      const savedOrders = JSON.parse(localStorage.getItem("orders")) || [];
      this.orders = savedOrders;
    },
  };
  </script>
  
  <style>
  .order-history {
    margin: 20px auto;
    padding: 20px;
    max-width: 100%;
    background-color: purple;
    color: white;
    border-radius: 10px;
  }
  
  .order {
    border: 2px solid gold; /* Добавлено оформление рамки для всех заказов */
    padding: 10px;
    margin-bottom: 20px;
    box-shadow: 0 10px 20px rgba(255, 215, 0, 0.5);
    background-color: rgba(255, 255, 0, 0.1);
  }
  
  .order ul {
    list-style: none;
    padding: 0;
  }
  
  .item-image {
    width: 220px;
    height: 125px;
    margin-right: 10px;
    border-radius: 10px;
    box-shadow: 0 10px 15px rgba(255, 0, 0, 0.5);
    object-fit: cover;
  }
  </style>
  