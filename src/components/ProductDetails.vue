<template>
    <div v-if="product" class="product-details">
      <h2>{{ product.name }}</h2>
      <img :src="product.image" alt="Product Image" class="product-image" />
      <p><strong>Описание:</strong> {{ product.description || 'Нет описания' }}</p>
      <p><strong>Цена:</strong> {{ product.price }} KZT за кг</p>
      <p><strong>Дата публикации:</strong> {{ product.date || 'Не указана' }}</p>
      <button @click="$emit('add-to-cart', product)" class="btn btn-primary">Добавить в корзину</button>
      <button @click="$emit('back-to-catalog')" class="btn btn-secondary">Вернуться к каталогу</button>
    </div>
  </template>
  
  <script>
  export default {
    name: "ProductDetails",
    props: {
      product: {
        type: Object,
        required: true,
      },
    },
  };
  </script>
  
   <style>
   .product-details {
     margin-bottom: 20px;
     box-shadow: 0 10px 10px rgba(127, 8, 255, 0.377);
     background-color: purple;
     border-radius: 10px
     
   }
   .product-list {
     display: flex;
     flex-wrap: wrap;
     gap: 20px;
     justify-content: center;
   }
   .product-details {
   box-shadow: 0 10px 10px rgba(127, 8, 255, 0.377);
   padding: 10px;
   flex-basis: calc(50% - 20px); 
   text-align: center;
   background-color: purple;
   border-radius: 10px;
   width: 1000px;
 }
 
 .product-image {
   width: 450px;
   height: 250px;
   object-fit: cover;
   border-radius: 10px;
   box-shadow: 0 0 15px rgba(127, 8, 255, 0.4); 
   transition: transform 0.3s ease, box-shadow 0.3s ease; 
 }
 
 .product img:hover {
   transform: scale(1.05); 
   box-shadow: 0 0 25px rgba(127, 8, 255, 0.6); 
 }
   </style>

  