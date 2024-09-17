<template>
    <v-container fluid>
        <!-- Кнопка удаления -->
        <v-btn fab color="red" @click="showDeleteForm = true" class="mb-4">
          <v-icon>mdi-delete</v-icon>
        </v-btn>
    

    <v-dialog v-model="showDeleteForm" max-width="400px">
      <v-card>
        <v-card-title>
          Удаление пользователя
        </v-card-title>
        <v-card-text>
          <v-text-field
            v-model="userId"
            label="ID пользователя"
            outlined
          />
          <v-alert v-if="errorMessage" type="error" class="mt-2">
            {{ errorMessage }}
          </v-alert>
        </v-card-text>
        <v-card-actions>
          <v-btn
            color="red"
            @click="deleteUser"
            :loading="isLoading"
          >
            Удалить
          </v-btn>
          <v-btn @click="showDeleteForm = false">Отмена</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    </v-container>
  </template>
  
  <script>
  export default {
    name: 'Delete_User',
    data() {
      return {
        showDeleteForm: false,
        userId: '', // ID пользователя для удаления
        isLoading: false,
        errorMessage: ''
      };
    },
    methods: {
      async deleteUser() {
        if (!this.userId) {
          this.errorMessage = 'Пожалуйста, введите ID пользователя.';
          return;
        }
  
        this.isLoading = true;
        try {
          const response = await fetch(`http://0.0.0.0:4040/api/v1/users/${this.userId}`, {
            method: 'DELETE',
            headers: {
              'Authorization': `Bearer ${localStorage.getItem('accessToken')}`,
              'Content-Type': 'application/json',
            },
          });
  
          if (response.ok) {
            this.$emit('user-deleted');
            this.userId = '';
            this.errorMessage = '';
            this.showDeleteForm = false; // Закрываем форму после успешного удаления
          } else {
            this.errorMessage = 'Ошибка при удалении пользователя.';
          }
        } catch (error) {
          console.error('Ошибка сети:', error);
          this.errorMessage = 'Ошибка при удалении пользователя. Попробуйте снова.';
        } finally {
          this.isLoading = false;
        }
      }
    }
  };
  </script>
  
  <style scoped>
  /* Стили для модального окна можно добавить здесь, если необходимо */
  </style>
  