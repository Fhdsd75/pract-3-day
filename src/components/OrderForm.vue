<template>
  <div class="order-form">
    <h2>Оформление заказа</h2>
    <form @submit.prevent="showConfirmation">
      <div class="form-group">
        <label for="name">Ф.И.О.</label>
        <input id="name" v-model="form.name" type="text" required />
      </div>
      <div class="form-group">
        <label for="phone">Номер телефона</label>
        <input
          id="phone"
          v-model="form.phone"
          type="text"
          required
          @input="formatPhone"
          placeholder="+77012345678"
        />
      </div>
      <div class="form-group">
        <label for="address">Адрес</label>
        <input id="address" v-model="form.address" type="text" required />
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
      </div>
    </form>

    <h3>Ваш заказ:</h3>
    <ul>
      <li v-for="item in cartItems" :key="item.id" class="cart-item">
        <img :src="item.image" alt="Фото товара" class="item-image" />
        <div class="item-info">
          {{ item.name }} - {{ item.quantity }} шт. ({{ item.price * item.quantity }} KZT)
        </div>
      </li>
    </ul>

    <confirmation-modal
      v-if="showModal"
      :form="form"
      :cart-items="cartItems"
      :total-cost="totalCost"
      @confirm="submitOrder"
      @cancel="closeModal"
    />
  </div>
</template>


<script>
import ConfirmationModal from "./ConfirmationModal.vue";

export default {
  name: "OrderForm",
  components: {
    ConfirmationModal,
  },
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
        phone: "",
        address: "",
        delivery: "",
        payment: "",
      },
      showModal: false,
    };
  },
  computed: {
    totalCost() {
      return this.cartItems.reduce((sum, item) => sum + item.price * item.quantity, 0);
    },
  },
  methods: {
    showConfirmation() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
    submitOrder() {
      const newOrder = {
        form: this.form,
        cartItems: this.cartItems,
        totalCost: this.totalCost,
        date: new Date().toLocaleString(),
      };
      const orders = JSON.parse(localStorage.getItem("orders")) || [];
      orders.push(newOrder);
      localStorage.setItem("orders", JSON.stringify(orders));

      alert("Заказ подтвержден!");
      this.$emit("order-completed");
      this.showModal = false;
    },
    formatPhone() {
      this.form.phone = this.form.phone.replace(/[^\d+]/g, "").slice(0, 12);
      if (!this.form.phone.startsWith("+")) {
        this.form.phone = "+" + this.form.phone.replace(/\D/g, "");
      }
    },
  },
};

</script>

<style scoped>
.order-form {
  margin: 20px auto;
  padding: 20px;
  max-width: 600px;
  background-color: purple;
  color: white;
  border-radius: 10px;
  box-shadow: 0 10px 15px rgba(127, 8, 255, 0.5);
}

.order-form h2 {
  text-align: center;
  color: #ffd700;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #ffd700;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.1);
  font-size: 16px;
}

.form-group input:focus,
.form-group select:focus {
  outline: none;
  border-color: #ffd700;
  box-shadow: 0 0 5px rgba(255, 215, 0, 0.5);
}

.buttons {
  text-align: center;
  margin-top: 20px;
}

.buttons .btn-success {
  background-color: #4caf50;
  color: white;
  font-size: 18px;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.buttons .btn-success:hover {
  background-color: #45a049;
  box-shadow: 0 5px 10px rgba(72, 239, 64, 0.3);
}

/* Стили списка товаров */
.cart-item {
  display: flex;
  align-items: center;
  margin-bottom: 15px;
  padding: 10px;
  background-color: #282828;
  border-radius: 8px;
}

.item-image {
  max-width: 100px;
  height: 100px;
  margin-right: 15px;
  border-radius: 8px;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.3);
  object-fit: cover;
}

.item-info {
  flex-grow: 1;
  color: #fff;
}

</style>

