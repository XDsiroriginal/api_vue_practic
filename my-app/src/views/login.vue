<script>

import axios from "axios";

export default {
  name: 'login',

  data() {
    return {
      email: '',
      password: '',
      login: '',
      error: null,
      isAuthenticate: false,
    }
  },

  methods: {

    async handleLogin() {
      try {
        const response = await axios.post(`${process.env.VUE_APP_API_URL}/login`, {
          email: this.email,
          password: this.password
        }, {
          headers: {
            'Content-Type': 'application/json'
          }
        });

        if (response.status === 200) {
          const token = response.data.data.user_token;

          this.login = this.email;
          this.isAuthenticate = true;
          this.saveToken(token);
          this.$router.push('/');
        }

      } catch (error) {
        console.log('ошибка отправки post запроса на сервер')

        if (error.response) {
          if (error.response.status === 401) {
            this.error = 'Неверный email или пароль';
          } else {
            this.error = 'Ошибка сервера. Попробуйте позже';
          }
        } else {
          this.error = 'Ошибка сети. Проверьте подключение';
        }
      }

    },

    saveToken(token) {
      localStorage.setItem('auth_token', token);

      localStorage.setItem('user_login', this.login);

      axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
    },
  },

  async mounted() {
    const token = localStorage.getItem('auth_token');
    const login = localStorage.getItem('user_login');
    if (token && login) {
      axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;

      try {
        this.$router.push('/');
      } catch (error) {
        localStorage.removeItem('auth_token');
        localStorage.removeItem('user_login')
      }
    }
  }
}

</script>

<template>
  <div class="login-container">
    <div class="login-card">
      <h2 class="login-title">Вход в систему</h2>

      <form @submit.prevent="handleLogin" class="login-form">
        <div class="form-group">
          <label for="email" class="form-label">Email</label>
          <input
              type="email"
              id="email"
              v-model="email"
              class="form-input"
              placeholder="Введите ваш email"
              required
          />
        </div>

        <div class="form-group">
          <label for="password" class="form-label">Пароль</label>
          <input
              type="password"
              id="password"
              v-model="password"
              class="form-input"
              placeholder="Введите ваш пароль"
              required
          />
        </div>

        <div v-if="error" class="error-message">
          {{ error }}
        </div>

        <button type="submit" class="login-btn">
          Войти
        </button>
      </form>

    </div>
  </div>
</template>

<style scoped>

.login-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 20px;
}

.login-card {
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
  padding: 40px;
  width: 100%;
  max-width: 400px;
}

.login-title {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
  font-size: 28px;
  font-weight: 600;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-label {
  font-size: 14px;
  font-weight: 500;
  color: #555;
}

.form-input {
  padding: 12px 16px;
  border: 2px solid #e1e1e1;
  border-radius: 8px;
  font-size: 16px;
  transition: all 0.3s ease;
  outline: none;
}

.form-input:focus {
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.form-input::placeholder {
  color: #aaa;
}

.error-message {
  background-color: #fee2e2;
  color: #dc2626;
  padding: 12px 16px;
  border-radius: 8px;
  font-size: 14px;
  border-left: 4px solid #dc2626;
}

.success-message {
  background-color: #d1fae5;
  color: #059669;
  padding: 12px 16px;
  border-radius: 8px;
  font-size: 14px;
  border-left: 4px solid #059669;
  margin-top: 20px;
  text-align: center;
}

.login-btn {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 14px 24px;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 10px;
}

.login-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
}

.login-btn:active {
  transform: translateY(0);
}

.login-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

</style>