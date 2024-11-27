<template>
  <div id="app">
    <h1>Каталог фруктов и овощей</h1>
    <Catalog
      v-if="!selectedProduct && !showCart && !showOrderForm"
      :products="products"
      @view-details="viewDetails"
      @add-to-cart="addToCart"
    />
    <ProductDetails
      v-else-if="selectedProduct"
      :product="selectedProduct"
      @add-to-cart="addToCart"
      @back-to-catalog="resetView"
    />
    <Cart
      v-else-if="showCart"
      :cart-items="cart"
      @remove-from-cart="removeFromCart"
      @checkout="openOrderForm"
    />
    <OrderForm
      v-else
      :cart-items="cart"
      @remove-from-cart="removeFromCart"
      @update-quantity="updateQuantity"
      @order-completed="completeOrder"
      @order-cancelled="cancelOrder"
    />
    <div class="nav-buttons">
      <button @click="resetView" class="btn btn-secondary">К списку товаров</button>
      <button @click="showCart = true" class="btn btn-primary">Корзина ({{ cart.length }})</button>
    </div>
  </div>
</template>

<script>
import Catalog from "./components/Catalog.vue";
import ProductDetails from "./components/ProductDetails.vue";
import Cart from "./components/Cart.vue";
import OrderForm from "./components/OrderForm.vue";

export default {
  name: "App",
  components: {
    Catalog,
    ProductDetails,
    Cart,
    OrderForm,
  },
  data() {
    return {
      products: [
        {
          id: 1,
          name: "Яблоки",
          category: "Фрукты",
          price: 500,
          image: "https://s0.rbk.ru/v6_top_pics/media/img/2/59/346834580613592.jpg",
          description: "Свежие красные яблоки из сада. Отличный выбор для перекуса или приготовления десертов.",
          date: "2024-11-01",
        },
        {
          id: 2,
          name: "Бананы",
          category: "Фрукты",
          price: 700,
          image: "https://www.m24.ru/b/d/nBkSUhL2hFcmn863Ir6BrNOp2Z318Ji-mifGnuWR9mOBdDebBizCnTY8qdJf6ReJ58vU9meMMok3Ee2nhSR6ISeO9G1N_wjJ=Iu86P-XOY9QrVqnZjlOrcw.jpg",
          description: "Спелые и сладкие бананы из Эквадора. Богаты витаминами и минералами.",
          date: "2024-11-02",
        },
        {
          id: 3,
          name: "Морковь",
          category: "Овощи",
          price: 300,
          image: "https://avatars.mds.yandex.net/i?id=a26d8323f455e39898d3cc763fd03b88_l-13101691-images-thumbs&n=13",
          description: "Сочная морковь, идеально подходящая для супов и салатов. Богатый источник витамина A.",
          date: "2024-11-03",
        },
        {
          id: 4,
          name: "Картофель",
          category: "Овощи",
          price: 200,
          image: "https://cdn.culture.ru/images/93749d1d-4140-54b0-8f9e-755c3996ba25",
          description: "Картофель сорта 'Беллароза'. Отлично подходит для жарки и запекания.",
          date: "2024-11-04",
        },
        {
          id: 5,
          name: "Апельсины",
          category: "Фрукты",
          price: 600,
          image: "https://i.pinimg.com/736x/f0/74/6f/f0746f7be25001f911af2bc995473bbc.jpg",
          description: "Сочные и сладкие апельсины из Испании. Идеальны для соков и десертов.",
          date: "2024-11-05",
        },
        {
          id: 6,
          name: "Огурцы",
          category: "Овощи",
          price: 400,
          image: "https://avatars.mds.yandex.net/i?id=3ccffdafac47bf4de6fd90c6fc7bda85e9801528fd2a6bdf-12532493-images-thumbs&n=13",
          description: "Хрустящие свежие огурцы. Подходят для салатов и маринования.",
          date: "2024-11-06",
        },
      ],
      selectedProduct: null,
      cart: [],
      showCart: false,
      showOrderForm: false,
    };
  },
  methods: {
    viewDetails(product) {
      this.selectedProduct = product;
    },
    addToCart(product) {
      const item = this.cart.find((p) => p.id === product.id);
      if (item) {
        item.quantity += 1;
      } else {
        this.cart.push({ ...product, quantity: 1 });
      }
    },
    removeFromCart(productId) {
      this.cart = this.cart.filter((item) => item.id !== productId);
    },
    updateQuantity(item) {
      const product = this.cart.find((p) => p.id === item.id);
      if (product) {
        product.quantity = item.quantity;
      }
    },
    openOrderForm() {
      this.showCart = false;
      this.showOrderForm = true;
    },
    completeOrder() {
      alert("Спасибо за покупку! Ваш заказ оформлен.");
      this.cart = [];
      this.showOrderForm = false;
    },
    cancelOrder() {
      this.showOrderForm = false;
      this.showCart = true;
    },
    resetView() {
      this.selectedProduct = null;
      this.showCart = false;
      this.showOrderForm = false;
    },
  },
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
}

.nav-buttons {
  margin-top: 20px;
}
</style>

<style>
.nav-buttons{
  margin-top: 20px;
  margin-bottom: 20px;
    box-shadow: 0 10px 10px rgba(127, 8, 255, 0.377);
    background-color: purple;
    border-radius: 10px
}
#app {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center; 
  font-family: "Arial", sans-serif;
  color: #e0b3ff; 
  padding: 20px;
  min-height: 100vh;
}

h1 {
  font-size: 4rem; 
  font-weight: bold; 
  color: #e0b3ff; 
  text-shadow: 
    0 0 5px #e0b3ff, 
    0 0 10px #d17bff, 
    0 0 20px #b34dff; 
  margin: 0; 
  transition: transform 0.3s ease-in-out; 
}

h1:hover {
  transform: scale(1.1);
  text-shadow: 
    0 0 10px #e0b3ff, 
    0 0 20px #d17bff, 
    0 0 40px #b34dff; 
}

</style>
