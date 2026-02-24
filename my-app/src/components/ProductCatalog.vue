<script>

  import axios from 'axios'

  export default {
    name: 'ProductCatalog',

    data() {
      return {
        products: [],
        isAuthenticate: false,
        token: '',
        login: '',
      }
    },

    methods: {
      logout() {
        localStorage.removeItem('auth_token');
        localStorage.removeItem('user_login');
        this.token = '';
        this.login = '';
        this.isAuthenticate = false;
      },
    },

    async mounted() {
      try {
        const response = await axios.get(`${process.env.VUE_APP_API_URL}/products`)
        console.log(response.data.data)

        this.products = response.data.data

      } catch (error) {
        console.log('ошибка отправки get запроса на сервер')
      }

      this.token = localStorage.getItem('auth_token');
      this.login = localStorage.getItem('user_login');
      if (this.token) {
        this.isAuthenticate = true;
      }
    },

  }


</script>

<template>
  <div class="product-catalog">
    <div class="auth-panel">
      <template v-if="!isAuthenticate">
        <div class="auth-links">
          <router-link to="/login" class="auth-link">Вход</router-link>
          <router-link to="/register" class="auth-link">Регистрация</router-link>
        </div>
      </template>
      <template v-else>
        <div class="user-info">
          <span class="user-login">Привет, {{ login }}</span>
          <button @click="logout" class="logout-btn">Выход</button>
        </div>
      </template>
    </div>

    <div class="products-grid">
      <div v-for="product in products" :key="product.id" class="product-card">
        <div class="product-image">
          <img :src="'http://lifestealer86.ru/' + product.image" :alt="product.name">
        </div>
        <div class="product-info">
          <h3 class="product-name">{{ product.name }}</h3>
          <p class="product-description">{{ product.description }}</p>
          <div class="product-price">{{ product.price }} ₽</div>
          <button class="add-to-cart-btn">
            В корзину
          </button>
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped>
.auth-panel {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding: 10px 0;
  margin-bottom: 20px;
  border-bottom: 1px solid #eaeaea;
}

.auth-links {
  display: flex;
  gap: 15px;
}

.auth-link {
  text-decoration: none;
  color: #3498db;
  font-weight: 500;
  font-size: 14px;
  transition: color 0.2s ease;
}

.auth-link:hover {
  color: #2980b9;
  text-decoration: underline;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 15px;
}

.user-login {
  font-size: 14px;
  color: #333;
  font-weight: 600;
}

.logout-btn {
  padding: 6px 12px;
  background: #e74c3c;
  color: #ffffff;
  border: none;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.3s ease;
}

.logout-btn:hover {
  background: #c0392b;
}

.product-catalog {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 24px;
}

.product-card {
  background: #ffffff;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  flex-direction: column;
}

.product-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
}

.product-image {
  width: 100%;
  height: 200px;
  overflow: hidden;
  background: #f5f5f5;
}

.product-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.product-info {
  padding: 16px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.product-name {
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin: 0 0 12px 0;
  line-height: 1.4;
}

.product-description {
  font-size: 14px;
  color: #666;
  margin: 0 0 16px 0;
  line-height: 1.5;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  flex: 1;
}

.product-price {
  font-size: 20px;
  font-weight: 700;
  color: #2ecc71;
  margin-bottom: 16px;
}

.add-to-cart-btn {
  width: 100%;
  padding: 12px 20px;
  background: #3498db;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s ease, transform 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.add-to-cart-btn:hover {
  background: #2980b9;
  transform: scale(1.02);
}

.add-to-cart-btn:active {
  transform: scale(0.98);
}

.btn-icon {
  font-size: 18px;
}
</style>