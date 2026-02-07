<script setup>
defineProps({
  users: Array,
  loading: Boolean
})

const emit = defineEmits(['edit', 'delete'])
</script>

<template>
  <div v-if="loading" class="state-container">
    <div class="spinner"></div>
    <p>Cargando registros...</p>
  </div>

  <div v-else-if="users.length === 0" class="state-container">
    <p>No hay usuarios registrados actualmente.</p>
  </div>

  <div v-else class="table-wrapper">
    <table class="user-table">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Usuario</th>
          <th>Email</th>
          <th>Teléfono</th>
          <th class="text-center">Acciones</th>
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
  </div>
</template>

<style scoped>

.state-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px;
  background-color: #f8f9fa;
  border-radius: 8px;
  color: #666;
  font-weight: 500;
  text-align: center;
}

.spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #004481; 
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: spin 1s linear infinite;
  margin-bottom: 15px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}


.table-wrapper {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.05);
  width: 100%;
  
  
  overflow-x: auto;  
  -webkit-overflow-scrolling: touch; 
}

.user-table {
  width: 100%;
  border-collapse: collapse;
  
  
  min-width: 800px; 
}

.user-table th, .user-table td {
  padding: 14px 20px;
  text-align: left;
  border-bottom: 1px solid #eee;
  white-space: nowrap; 
}

.user-table th {
  background-color: #004481; 
  color: white;
  font-weight: 600;
  text-transform: uppercase;
  font-size: 0.85rem;
  letter-spacing: 0.5px;
}

.user-table tr:hover {
  background-color: #f8f9fa;
}

.text-center { text-align: center; }

.actions {
  display: flex;
  gap: 8px;
  justify-content: center;
}

button {
  padding: 6px 14px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.85rem;
  font-weight: 600;
  transition: all 0.2s;
}

.btn-edit {
  background-color: #e3f2fd;
  color: #1976d2;
}

.btn-edit:hover { background-color: #bbdefb; }

.btn-delete {
  background-color: #ffebee;
  color: #c62828;
}

.btn-delete:hover { background-color: #ffcdd2; }
</style>