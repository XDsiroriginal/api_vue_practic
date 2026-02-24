<script>

  import axios from 'axios'

  export default {
    name: 'ProductCatalog',

    data() {
      return {
        products: []
      }
    },

    async mounted() {
      try {
        const response = await axios.get(`${process.env.VUE_APP_API_URL}/products`)
        console.log(response.data.data)

        this.products = response.data.data

      } catch (error) {
        console.log('ошибка отправки get запроса на сервер')
      }
    },
  }


</script>

<template>

  <div class="product-catalog">
    <div class="products-grid">
      <div
          v-for="product in products"
          :key="product.id"
          class="product-card"
      >
        <div class="product-image">
          <img :src="'http://lifestealer86.ru/' + product.image" :alt="product.name" />
        </div>
        <div class="product-info">
          <h3 class="product-name">{{ product.name }}</h3>
          <p class="product-description">{{ product.description }}</p>
          <div class="product-price">{{ product.price }} ₽</div>
        </div>
      </div>
    </div>
  </div>

</template>

<style scoped>

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
}

.product-price {
  font-size: 20px;
  font-weight: 700;
  color: #2ecc71;
}

</style>