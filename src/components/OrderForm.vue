<template>
    <div class="order-form">
      <h2>Оформление заказа</h2>
      <form @submit.prevent="submitOrder">
        <div class="form-group">
          <label for="name">Ф.И.О.</label>
          <input id="name" v-model="form.name" type="text" required />
        </div>
  
        <div class="form-group">
          <label for="gender">Пол</label>
          <select id="gender" v-model="form.gender" required>
            <option value="male">Мужской</option>
            <option value="female">Женский</option>
          </select>
        </div>
  
        <div class="form-group">
          <label for="country">Страна</label>
          <select id="country" v-model="form.country" required>
            <option value="Казахстан">Казахстан</option>
            <option value="Россия">Россия</option>
            <option value="Беларусь">Беларусь</option>
          </select>
        </div>
  
        <div class="form-group">
          <label for="address">Адрес</label>
          <input id="address" v-model="form.address" type="text" required />
        </div>
  
        <div class="form-group">
          <label for="phone">Телефон</label>
          <input id="phone" v-model="form.phone" type="text" required />
        </div>
  
        <div class="form-group">
          <label for="delivery">Способ доставки</label>
          <select id="delivery" v-model="form.delivery" required>
            <option value="Самовывоз">Самовывоз</option>
            <option value="Курьер">Курьер</option>
            <option value="Почта">Почта</option>
          </select>
        </div>
  
        <div class="form-group">
          <label for="payment">Способ оплаты</label>
          <select id="payment" v-model="form.payment" required>
            <option value="Наличными">Наличными</option>
            <option value="Картой">Картой</option>
            <option value="Онлайн-оплата">Онлайн-оплата</option>
          </select>
        </div>
  
        <p><strong>Общая стоимость:</strong> {{ totalCost }} KZT</p>
  
        <div class="buttons">
          <button type="submit" class="btn btn-success">Подтвердить заказ</button>
          <button type="button" @click="cancelOrder" class="btn btn-danger">Отменить заказ</button>
        </div>
      </form>
  
      <h3>Ваш заказ:</h3>
      <div class="cart-items">
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
              <td><img :src="item.image" alt="Product Image" /></td>
              <td>{{ item.price }} KZT за кг</td>
              <td>
                <input
                  type="number"
                  v-model.number="item.quantity"
                  min="1"
                  @input="updateQuantity(item)"
                />
              </td>
              <td>
                <button @click="removeItem(item.id)" class="btn btn-danger">Удалить</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "OrderForm",
    props: {
      cartItems: {
        type: Array,
        required: true,
      },
    },
    data() {
      return {
        form: {
          name: "",
          gender: "",
          country: "",
          address: "",
          phone: "",
          delivery: "",
          payment: "",
        },
      };
    },
    computed: {
      totalCost() {
        return this.cartItems.reduce((sum, item) => sum + item.price * item.quantity, 0);
      },
    },
    methods: {
      submitOrder() {
        if (this.cartItems.length === 0) {
          alert("Корзина пуста! Добавьте товары перед оформлением заказа.");
          return;
        }
        alert("Ваш заказ успешно оформлен!");
        this.$emit("order-completed");
      },
      cancelOrder() {
        this.$emit("order-cancelled");
      },
      removeItem(productId) {
        this.$emit("remove-from-cart", productId);
      },
      updateQuantity(item) {
        if (item.quantity < 1) {
          item.quantity = 1;
        }
        this.$emit("update-quantity", item);
      },
    },
  };
  </script>
  
  <style scoped>

  .order-form {
    margin: 20px auto;
    padding: 20px;
    max-width: 100%; 
    background-color: purple;
    color: white;
    border-radius: 10px;
    box-shadow: 0 10px 15px rgba(127, 8, 255, 0.4); 
  }
  .form-group {
    margin-bottom: 20px;
    padding: 10px;
    background-color: purple;
    border-radius: 8px;
    box-shadow: 0 10px 10px rgba(127, 8, 255, 0.377);
  }
  
  input[type="text"], select {
    width: 100%;
    padding: 10px;
    font-size: 16px;
    border-radius: 5px;
    border: 2px solid #b19cd9;
    background-color: #f4e3ff;
    color: #4b0082;
  }
  
  select {
    background-color: #6a0dad;
    color: white;
  }
  
  input[type="text"]:focus, select:focus {
    outline: none;
    border-color: #8a2be2;
    box-shadow: 0 0 5px #8a2be2;
  }
  .cart-items {
    margin-top: 30px;
    padding: 10px;
    background-color: rgb(113, 2, 115);
    border-radius: 8px;
    box-shadow: 0 10px 10px rgba(127, 8, 255, 0.377);
  }
  
  .cart-items table {
    width: 100%;
    border-collapse: collapse;
    color: white;
  }
  
  .cart-items th, .cart-items td {
    padding: 10px;
    text-align: left;
  }
  
  .cart-items img {
    max-width: 80px;
    height: auto;
  }
  .buttons {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
  }
  
  .btn {
    padding: 10px 20px;
    border-radius: 5px;
    border: none;
    font-size: 16px;
  }
  
  .btn-success {
    background-color: #4caf50;
    color: white;
  }
  
  .btn-danger {
    background-color: #f44336;
    color: white;
  }
  </style>
  