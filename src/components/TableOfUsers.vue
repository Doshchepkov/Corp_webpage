<template>
  <v-container fluid>
    <v-row>
      <v-col cols="auto">
        <UserCreationForm
          :showDialog="showCreateDialog"
          @user-created="fetchUsers"
          @update:showDialog="showCreateDialog = $event"
        />
      </v-col>

      <v-col cols="auto">
        <Delete_User @user-deleted="fetchUsers" />
      </v-col>


      <v-col cols="auto">
        <Edit_User />
      </v-col>
      <v-col cols="auto" offset-md="8">
    <targ_search @filter-changed="applyFilter" />
  </v-col>


</v-row>
       


    <div id="app">
      <v-app id="inspire">
        <v-data-table
      dense
      :headers="headers"
      :items="filteredUsers"
      item-key="id"
      class="elevation-1"
    >
      <!-- Вывод данных таблицы -->
    </v-data-table>
      </v-app>
    </div>
  </v-container>
</template>

<script>
import UserCreationForm from './UserCreationForm.vue';
import Delete_User from './Delete_User.vue';
import Edit_User from './Edit_User.vue';
import targ_search from './targ_search.vue';

export default {
  name: 'TableOfUsers',
  components: {
    UserCreationForm, Delete_User, Edit_User, targ_search
  },
  data() {
    return {
      showCreateDialog: false,
      users: [],
      filteredUsers: [],
      searchQuery: '',
      headers: [
        { text: 'ID', value: 'id' },
        { text: 'Логин', value: 'login' },
        { text: 'Имя', value: 'name' },
        { text: 'Роль', value: 'role_id' },
        { text: 'Статус', value: 'status' }
      ]
    };
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await fetch('http://0.0.0.0:4040/api/v1/users', {
          method: 'GET',
          headers: {
            'Authorization': `Bearer ${localStorage.getItem('accessToken')}`,
            'Content-Type': 'application/json',
          },
        });

        if (response.ok) {
          this.users = await response.json();
          this.filteredUsers = this.users; // Изначально показываем все пользователи
        } else {
          console.error('Ошибка получения данных:', response.statusText);
        }
      } catch (error) {
        console.error('Ошибка сети:', error);
      }
    },
    applyFilter(query) {
      this.searchQuery = query;
      this.filteredUsers = this.users.filter(user => {
        return Object.keys(user).some(key =>
          String(user[key]).toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      });
    }
  },
  mounted() {
    this.fetchUsers(); // Загружаем пользователей при монтировании компонента
  }
};
</script>

<style scoped>
/* Стили для таблицы и кнопок */
</style>