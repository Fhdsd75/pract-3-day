<template>
    <div>
      <div class="filters">
        <select v-model="selectedCategory">
          <option value="">Все категории</option>
          <option v-for="category in categories" :key="category" :value="category">
            {{ category }}
          </option>
        </select>
        <input
          type="text"
          v-model="searchQuery"
          placeholder="Поиск по названию"
        />
        <button @click="resetFilters" :disabled="!isFiltered" class="btn btn-warning">Сбросить фильтр</button>
      </div>
  
      <div class="product-list">
        <div
          v-for="product in filteredProducts"
          :key="product.id"
          class="product"
        >
          <h3>{{ product.name }}</h3>
          <img :src="product.image" alt="Product Image" />
          <p>Категория: {{ product.category }}</p>
          <p>Цена: {{ product.price }} KZT за кг</p>
          <button @click="$emit('add-to-cart', product)" class="btn btn-primary">Добавить в корзину</button>
          <button @click="$emit('view-details', product)" class="btn btn-dark">Детали</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "Catalog",
    props: {
      products: {
        type: Array,
        required: true,
      },
    },
    data() {
      return {
        selectedCategory: "",
        searchQuery: "",
      };
    },
    computed: {
      filteredProducts() {
        return this.products.filter((product) => {
          const matchesCategory =
            !this.selectedCategory || product.category === this.selectedCategory;
          const matchesSearch =
            !this.searchQuery ||
            product.name.toLowerCase().includes(this.searchQuery.toLowerCase());
          return matchesCategory && matchesSearch;
        });
      },
      isFiltered() {
        return this.selectedCategory || this.searchQuery;
      },
    },
    methods: {
      resetFilters() {
        this.selectedCategory = "";
        this.searchQuery = "";
      },
    },
  };
  </script>
  
  <style>
  .filters {
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
  .product {
  box-shadow: 0 10px 10px rgba(127, 8, 255, 0.377);
  padding: 10px;
  flex-basis: calc(50% - 20px);
  text-align: center;
  background-color: purple;
  border-radius: 10px;
  width: 1000px;
}

.product img {
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


  input[type="text"] {
  background-color: #f4e3ff;
  border: 2px solid #b19cd9;
  border-radius: 5px;
  color: #4b0082; 
  padding: 10px; 
  font-size: 16px;
  outline: none; 
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

input[type="text"]::placeholder {
  color: #8a2be2; 
  font-style: italic;
}

input[type="text"]:focus {
  border-color: #8a2be2;
  box-shadow: 0 0 5px #8a2be2;
}
select {
  background-color: #6a0dad;
  color: white; 
  border: 2px solid #5c0ca8;
  border-radius: 5px; 
  padding: 10px;
  font-size: 16px; 
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.2); 
  appearance: none; 
  cursor: pointer;
  width: 150px;
}

select:focus {
  outline: none;
  border-color: #8a2be2;
  box-shadow: 0px 0px 8px rgba(138, 43, 226, 0.7); 
}

option {
  background-color: #6a0dad; 
  color: white;
}

option:hover {
  background-color: #8a2be2; 
}


  </style>
  