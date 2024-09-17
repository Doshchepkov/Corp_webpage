<template>
    <v-container fluid>
    <v-btn fab color="yellow" @click="showEditForm = true" class="mb-4">
          <v-icon>mdi-pencil</v-icon>
        </v-btn>
  <v-dialog v-model="showEditForm" max-width="600px">
    <v-card>
      <v-card-title>
        Редактирование пользователя
      </v-card-title>
      <v-card-text>
        <v-text-field v-model="id" label="ID" />
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
      </v-card-text>
      <v-card-actions>
        <v-btn color="primary" @click="updateUser">Сохранить</v-btn>
        <v-btn @click="showEditForm = false">Отмена</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</v-container>
</template>

<script>
export default {
  name: 'Edit_User',
  data() {
    return {
      showEditForm: false,
      id: '',
      username: '',
      password: '',
      name: '',
      roleId: null,
      roles: [
        { id: 1, name: 'supervisor' },
        { id: 2, name: 'developer' },
        { id: 3, name: 'customer' }
      ]
    };
  },
  methods: {
    async updateUser() {
      const data = {
        password: this.password,
        name: this.name,
        role_id: this.roleId
      };
      
      try {
        const response = await fetch(`http://0.0.0.0:4040/api/v1/users/${this.id}`, {
          method: 'PATCH',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${localStorage.getItem('accessToken')}`
          },
          body: JSON.stringify(data)
        });

        if (response.ok) {
          const result = await response.json();
          this.showEditForm = false;
          this.$emit('user-updated', result); // Уведомление о обновлении
        } else {
          console.error('Ошибка при обновлении пользователя.');
        }
      } catch (error) {
        console.error('Ошибка сети:', error);
      }
    }
  }
};
</script>

<style scoped>
/* Стили для Edit_User */
</style>
