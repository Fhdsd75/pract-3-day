<template>
    <div class="cart">
      <h2>Корзина</h2>
      <div v-if="cartItems.length">
        <table>
          <thead>
            <tr>
              <th>Название</th>
              <th>Изображение</th>
              <th>Цена</th>
              <th>Количество</th>
              <th>Действия</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in cartItems" :key="item.id">
              <td>{{ item.name }}</td>
              <td><img :src="item.image" alt="Product Image" class="product-image" /></td>
              <td>{{ item.price }} KZT за кг</td>
              <td>
                <input type="number" v-model.number="item.quantity" min="1" />
              </td>
              <td>
                <button @click="$emit('remove-from-cart', item.id)" class="btn btn-danger">Удалить</button>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="total">
          <p><strong>Общая стоимость:</strong> {{ totalCost }} KZT </p>
          <button @click="$emit('checkout')" class="btn btn-success">Оформить заказ</button>
        </div>
      </div>
      <p v-else>Корзина пуста.</p>
    </div>
  </template>
  
  <script>
  export default {
    name: "Cart",
    props: {
      cartItems: {
        type: Array,
        required: true,
      },
    },
    computed: {
      totalCost() {
        return this.cartItems.reduce((sum, item) => sum + item.price * item.quantity, 0);
      },
    },
  };
  </script>
  
  <style scoped>
  .cart {
    margin: 20px auto;
    padding: 20px;
    max-width: auto;
    background-color: purple;
    color: white;
    border-radius: 10px;
    box-shadow: 0 10px 15px rgba(127, 8, 255, 0.4);
  }
  
  h2 {
    text-align: center;
    margin-bottom: 20px;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  th, td {
    padding: 10px;
    text-align: center;
    border-bottom: 1px solid #ccc;
  }
  
  th {
    background-color: purple;
    color: white;
  }
  
  td img {
    max-width: auto;
    height: auto;
    border-radius: 5px;
  }
  
  input[type="number"] {
    width: 60px;
    padding: 5px;
    text-align: center;
    border: 2px solid #b19cd9;
    border-radius: 5px;
  }
  
  input[type="number"]:focus {
    border-color: #8a2be2;
    outline: none;
  }
  
  button {
    padding: 5px 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s ease;
  }
  
  button:hover {
    transform: scale(1.05);
  }
  
  .btn-danger {
    background-color: #ff4d4d;
    color: white;
  }
  
  .btn-danger:hover {
    background-color: #ff1a1a;
  }
  
  .btn-success {
    background-color: #28a745;
    color: white;
  }
  
  .btn-success:hover {
    background-color: #218838;
  }
  
  .total {
    margin-top: 20px;
    text-align: right;
  }
  
  .total p {
    font-size: 18px;
    font-weight: bold;
  }
  
  .product-image {
    width: 450px;
    height: 250px;
    object-fit: cover;
    border-radius: 10px;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }
  
  .product-image:hover {
    transform: scale(1.1);
    box-shadow: 0 0 10px rgba(127, 8, 255, 0.6);
  }
  </style>
  