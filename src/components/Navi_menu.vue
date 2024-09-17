<template>
  <v-container data-app="true" class="container container--fluid">
    <v-app id="inspire">
      <v-app-bar color="blue accent-4" dense dark>
        <v-btn outlined class="ml-3" @click="showMainPage">Главная</v-btn>
        <v-btn outlined class="ml-3" @click="showTableOfUsers">Пользователи</v-btn>
        <v-spacer />
        <v-btn outlined class="ml-3" @click="logout">Выход</v-btn>
        
      </v-app-bar>

      <Main_page v-if="showMain" />
      <TableOfUsers v-if="showTable" />
    </v-app>
  </v-container>
</template> 

<script>
import Main_page from './Main_page.vue';
import TableOfUsers from './TableOfUsers.vue';

export default {
  name: 'navi_menu',
  components: {
    Main_page,
    TableOfUsers
  },
  data() {
    return {
      showTable: false,
      showMain: true
    };
  },
  methods: {
    showMainPage() {
      this.showMain = true;
      this.showTable = false;
    },
    showTableOfUsers() {
      this.showMain = false;
      this.showTable = true;
    },
    logout() {
      // Получаем токен доступа из локального хранилища
      const accessToken = localStorage.getItem('accessToken');

      // Проверяем, есть ли токен
      if (!accessToken) {
        console.error('Токен доступа отсутствует.');
        alert('Токен доступа отсутствует.');
        return;
      }

      // Выполняем запрос на выход
      fetch('http://0.0.0.0:4040/api/v1/users', {
          method: 'GET',
          headers: {
            'Authorization': 'Bearer ' + localStorage.getItem('accessToken'),
            'Content-Type': 'application/json',
          },
        })
      .then(response => {
        if (response.ok) {
          // Если запрос успешен, удаляем токен из локального хранилища и эмитируем событие
          localStorage.removeItem('accessToken');
          localStorage.removeItem('username'); // Если также хранится имя пользователя
          this.$emit('logout-success');
          alert('Выход выполнен успешно!');
          // Переключаемся обратно на экран входа
          this.showMainPage();
        } else {
          throw new Error('Ошибка выхода из системы');
        }
      })
      .catch(error => {
        console.error(error);
        alert('Ошибка выхода. Попробуйте еще раз.');
      });
    }
  }
};
</script>

<style scoped>
/* Стили для компонента навигационного меню */
</style>
