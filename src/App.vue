<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import UserTable from './components/UserTable.vue'
import UserModal from './components/UserModal.vue'

const users = ref([])
const loading = ref(false)
const error = ref('')
const showModal = ref(false)
const editingUser = ref(null)

const fetchUsers = async () => {
  loading.value = true
  error.value = ''
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/users')
    users.value = response.data
  } catch (err) {
    console.error(err)
    error.value = 'Hubo un error al cargar los usuarios. Por favor intenta más tarde.'
  } finally {
    loading.value = false
  }
}

const handleDelete = (id) => {
  if (window.confirm('¿Estás seguro de eliminar este usuario?')) {
    users.value = users.value.filter(user => user.id !== id)
  }
}

const handleEdit = (user) => {
  editingUser.value = user
  showModal.value = true
}

const handleCreate = () => {
  editingUser.value = null
  showModal.value = true
}

const handleSave = (userData) => {
  if (userData.id) {
    const index = users.value.findIndex(u => u.id === userData.id)
    if (index !== -1) {
      users.value[index] = userData
    }
  } else {
    const newId = users.value.length > 0 ? Math.max(...users.value.map(u => u.id)) + 1 : 1
    const newUser = { ...userData, id: newId }
    users.value.push(newUser)
  }
  showModal.value = false
}

onMounted(() => {
  fetchUsers()
})
</script>

<template>
  <div class="main-container">
    <header class="top-bar">
      <h1>Gestión de Usuarios</h1>
      <button @click="handleCreate" class="btn-create" :disabled="loading">
        + Nuevo Usuario
      </button>
    </header>

    <div v-if="error" class="error-banner">
      {{ error }}
    </div>

    <div class="table-responsive">
      <UserTable 
        :users="users" 
        :loading="loading" 
        @edit="handleEdit" 
        @delete="handleDelete" 
      />
    </div>

    <UserModal 
      :show="showModal" 
      :user-data="editingUser"
      @close="showModal = false" 
      @save="handleSave" 
    />
  </div>
</template>

<style scoped>
.main-container {
  max-width: 1000px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 20px;
  box-sizing: border-box;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.top-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
}

h1 {
  color: #2c3e50;
  margin: 0;
  font-size: 1.8rem;
}

.btn-create {
  background-color: #2ecc71;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
  font-size: 1rem;
  box-shadow: 0 2px 5px rgba(46, 204, 113, 0.3);
  transition: transform 0.1s, box-shadow 0.2s;
}

.btn-create:hover {
  background-color: #27ae60;
  box-shadow: 0 4px 8px rgba(46, 204, 113, 0.4);
}

.btn-create:active {
  transform: translateY(1px);
}

.btn-create:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
  box-shadow: none;
}

.error-banner {
  background-color: #ffebee;
  color: #c62828;
  padding: 12px;
  border-radius: 4px;
  margin-bottom: 20px;
  border: 1px solid #ef9a9a;
}

.table-responsive {
  width: 100%;
  overflow-x: auto;
}
</style>