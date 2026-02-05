<script setup>
defineProps({
  users: Array,
  loading: Boolean
})

const emit = defineEmits(['edit', 'delete'])
</script>

<template>
  <div v-if="loading" class="loading">
    Cargando datos...
  </div>

  <table v-else class="user-table">
    <thead>
      <tr>
        <th>Nombre</th>
        <th>Usuario</th>
        <th>Email</th>
        <th>Teléfono</th>
        <th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="user in users" :key="user.id">
        <td>{{ user.name }}</td>
        <td>{{ user.username }}</td>
        <td>{{ user.email }}</td>
        <td>{{ user.phone }}</td>
        <td>
          <div class="actions">
            <button @click="emit('edit', user)" class="btn-edit">Editar</button>
            <button @click="emit('delete', user.id)" class="btn-delete">Eliminar</button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
.loading {
  text-align: center;
  color: #666;
  margin-top: 20px;
  font-size: 1.2rem;
}

.user-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  background-color: white; 
}

.user-table th, .user-table td {
  border: 1px solid #e0e0e0;
  padding: 12px 15px;
  text-align: left;
}

.user-table th {
  background-color: #f8f9fa;
  font-weight: 600;
  color: #333;
}

.actions {
  display: flex;
  gap: 8px;
}

button {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: opacity 0.2s;
}

.btn-edit {
  background-color: #3498db;
  color: white;
}

.btn-delete {
  background-color: #e74c3c;
  color: white;
}

button:hover {
  opacity: 0.9;
}
</style>