<template>
  <v-app>
    <v-container fluid>
      <!-- Показать Auth_Screen только если showAuthScreen истинно -->
      <Auth_Screen v-if="showAuthScreen" @login-success="handleLoginSuccess" />

      <!-- Основной контент, который отображается после аутентификации -->
      <v-container v-if="!showAuthScreen" fluid>
        <!-- Другие компоненты -->
        <Navi_menu @logout-success="handleLogoutSuccess" />
      </v-container>
    </v-container>
  </v-app>
</template>

<script>
import Auth_Screen from './components/Auth_Screen.vue';
import Navi_menu from './components/Navi_menu.vue';

export default {
  name: 'App',
  components: {
    Auth_Screen,
    Navi_menu
  },
  data() {
    return {
      showAuthScreen: true
    };
  },
  methods: {
    handleLoginSuccess() {
      this.showAuthScreen = false;
    },
    handleLogoutSuccess() {
      this.showAuthScreen = true;
    }
  },
  mounted() {
    // Проверяем, есть ли токен при загрузке
    const accessToken = localStorage.getItem('accessToken');
    this.showAuthScreen = !accessToken;
  }
};
</script>

<style scoped>
/* Стили для основного компонента */
</style>
