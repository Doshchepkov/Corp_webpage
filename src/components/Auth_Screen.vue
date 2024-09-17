<template>
  <v-container
    class="auth-container"
    fluid
    fill-height
    align-center
    justify-center
  >
    <v-card class="auth-card" max-width="400px" outlined>
      <v-card-title class="text-h5 text-center">Вход в систему</v-card-title>

      <v-card-subtitle class="text-center">
        Пожалуйста, введите свои данные для входа
      </v-card-subtitle>
        
      <v-form ref="form" v-model="valid" lazy-validation>
        <v-card-text>
          <v-text-field
            v-model="username"
            :counter="32"
            :rules="nameRules"
            label="Логин"
            required
            class="mb-4"
            outlined
            color="blue lighten-3"
          ></v-text-field>

          <v-text-field
            v-model="password"
            :counter="32"
            :rules="nameRules"
            label="Пароль"
            required
            type="password"
            outlined
            color="blue lighten-3"
          ></v-text-field>
        </v-card-text>

        <v-card-actions>
          <v-btn
            color="blue lighten-3"
            class="white--text"
            @click="handleLogin"
          >
            Войти
          </v-btn>
        </v-card-actions>
      </v-form>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      password: '',
      valid: true,
      nameRules: [v => !!v || 'Обязательное поле.'],
    };
  },


  
  methods: {
    handleLogin() {
      let formData = new FormData();
      formData.append('username', this.username);
      formData.append('password', this.password);

      fetch('http://0.0.0.0:4040/api/v1/auth/token', {
        method: 'POST',
        body: formData
      })
      .then(response => {
        if (response.ok) {
          return response.json();
        } else {
          throw new Error('Ошибка аутентификации');
        }
      })
      .then(data => {
        localStorage.setItem('accessToken', data.access_token);
        localStorage.setItem('username', this.username);
        this.$emit('login-success');
        alert('Успешная аутентификация!');
      })
      .catch(error => {
        console.error(error);
        alert('Ошибка аутентификации. Проверьте логин и пароль.');
      });
    }
  }
};
</script>

<style scoped>
.auth-container {
  background-color: #f0f4f8;
  min-height: 100vh;
}

.auth-card {
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.auth-card .v-card-title {
  color: #0237bd;
}

.auth-card .v-card-subtitle {
  color: #555;
}
</style>
