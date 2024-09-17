<template>
  <v-container fluid>
    <!-- Кнопка для отображения формы создания пользователя -->
    <v-btn fab color="primary" @click="showCreateUserDialog = true" class="mb-4">
      <v-icon>mdi-plus</v-icon>
    </v-btn>

    <!-- Диалоговое окно создания пользователя -->
    <v-dialog v-model="showCreateUserDialog" max-width="600px">
      <v-card>
        <v-card-title>
          Создание пользователя
        </v-card-title>
        <v-card-text>
          <v-text-field v-model="username" label="Username" />
          <v-text-field v-model="password" label="Password" type="password" />
          <v-text-field v-model="name" label="Name" />
          <v-select
            v-model="roleId"
            :items="roles"
            item-text="name"
            item-value="id"
            label="Role"
          />
          
          <v-alert v-if="errorMessage" type="error">{{ errorMessage }}</v-alert>
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" @click="createUser" :disabled="isLoading">
            {{ isLoading ? 'Создание...' : 'Создать' }}
          </v-btn>
          <v-btn @click="showCreateUserDialog = false">Отмена</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: 'UserCreationForm',
  data() {
    return {
      showCreateUserDialog: false, // Управляет видимостью диалога
      username: '',
      password: '',
      name: '',
      roleId: '',
      roles: [ // Фиксированный список ролей
        { id: 1, name: 'supervisor' },
        { id: 2, name: 'developer' },
        { id: 3, name: 'customer' }
      ],
      errorMessage: '',
      isLoading: false
    };
  },
  methods: {
    async createUser() {
      if (!this.username || !this.password || !this.name || !this.roleId) {
        this.errorMessage = 'Пожалуйста, заполните все поля.';
        return;
      }
      
      const data = {
        login: this.username,
        password: this.password,
        name: this.name,
        role_id: this.roles[this.roleId-1]['name'],
      };
      console.log(data)
      this.isLoading = true;

      
        const response = await fetch('http://0.0.0.0:4040/api/v1/users/', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${localStorage.getItem('accessToken')}`,
          },
          body: JSON.stringify(data),
        });

        if (response.ok) {
          const result = await response.json();
          this.$emit('user-created', result);
          this.resetForm();
          this.showCreateUserDialog = false; // Закрываем диалог после успешного создания
        } else {
          this.errorMessage = 'Ошибка при создании пользователя. Проверьте данные и попробуйте снова.';
        }

     
    },
    resetForm() {
      this.username = '';
      this.password = '';
      this.name = '';
      this.roleId = null;
      this.errorMessage = '';
    },
    get_data(){
      console.log(this.roles[this.roleId-1]['name'])
    }
  }
};
</script>

<style scoped>
/* Стили для диалога */
.v-card {
  padding: 20px;
}

.v-card-title {
  font-weight: bold;
}

.v-card-actions {
  display: flex;
  justify-content: flex-end;
}

.v-btn {
  margin: 0 5px;
}

.v-alert {
  margin-bottom: 10px;
}
</style>
