<template>
    <div class="modal">
      <div class="modal-content">
        <h3>Подтвердите ваш заказ</h3>
        <p><strong>Ф.И.О.:</strong> {{ form.name }}</p>
        <p><strong>Пол:</strong> {{ form.gender === "male" ? "Мужской" : "Женский" }}</p>
        <p><strong>Адрес:</strong> {{ form.address }}</p>
        <p><strong>Способ доставки:</strong> {{ form.delivery }}</p>
        <p><strong>Способ оплаты:</strong> {{ form.payment }}</p>
        <h4>Товары:</h4>
        <ul>
          <li v-for="item in cartItems" :key="item.id" class="cart-item">
            <img :src="item.image" alt="Фото товара" class="item-image" />
            <div class="item-info">
              <p>{{ item.name }} - {{ item.quantity }} шт. ({{ item.price * item.quantity }} KZT)</p>
            </div>
          </li>
        </ul>
        <p><strong>Общая стоимость:</strong> {{ totalCost }} KZT</p>
        <div class="buttons">
  <button @click="handleCancel" class="btn btn-danger">Нет, изменить</button>
  <button @click="handleConfirm" class="btn btn-success">Да, всё верно</button>
</div>

      </div>
    </div>
  </template>
  
  <script>
export default {
  name: "ConfirmationModal",
  props: {
    form: {
      type: Object,
      required: true,
    },
    cartItems: {
      type: Array,
      required: true,
    },
    totalCost: {
      type: Number,
      required: true,
    },
  },
  methods: {
    handleCancel() {
      document.body.style.overflow = ""; 
      this.$emit("cancel"); 
    },
    handleConfirm() {
      document.body.style.overflow = ""; 
      this.$emit("confirm"); 
    },
  },
  mounted() {
    document.body.style.overflow = "hidden"; 
  },
  beforeDestroy() {
    document.body.style.overflow = ""; 
  },
};

  </script>
  
  <style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  overflow: hidden; 
}

.modal-content {
  background: rgb(204, 0, 255);
  padding: 20px;
  border-radius: 10px;
  width: 700px;
  max-height: 90vh; 
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.5);
  text-align: center;
  overflow-y: auto; 
}

.buttons {
  margin-top: 20px;
}

.btn {
  margin: 0 10px;
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.btn-success {
  background-color: #4caf50;
  color: white;
}

.btn-danger {
  background-color: #f44336;
  color: white;
}

.cart-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.item-image {
  max-width: 225px;
  height: 125px;
  margin-right: 10px;
  border-radius: 10px;
  box-shadow: 0 10px 15px rgba(255, 0, 0, 0.5);
  object-fit: cover;
}

.item-info {
  text-align: left;
}

  </style>
  