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

const showDeleteModal = ref(false)
const idToDelete = ref(null)
const isDeleting = ref(false)

const fetchUsers = async () => {
  loading.value = true
  error.value = ''

  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/users')
    
    setTimeout(() => {
      users.value = response.data
      loading.value = false
    }, 1500)

  } catch (err) {
    console.error(err)
    error.value = 'Hubo un error al cargar los usuarios. Por favor intenta más tarde.'
    loading.value = false
  }
}

const openDeleteModal = (id) => {
  idToDelete.value = id
  showDeleteModal.value = true
}

const executeDelete = () => {
  isDeleting.value = true 
  
  setTimeout(() => {
    users.value = users.value.filter(user => user.id !== idToDelete.value)
    isDeleting.value = false
    showDeleteModal.value = false
    idToDelete.value = null
  }, 1500) 
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
    const maxId = users.value.length > 0 ? Math.max(...users.value.map(u => u.id)) : 0
    const newUser = { ...userData, id: maxId + 1 }
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
      <div class="branding">
        <h1>Myper | Gestión de Usuarios</h1>
      </div>
      <button @click="handleCreate" class="btn-primary" :disabled="loading">
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
        @delete="openDeleteModal" 
      />
    </div>

    <UserModal 
      :show="showModal" 
      :user-data="editingUser"
      @close="showModal = false" 
      @save="handleSave" 
    />

    <div v-if="showDeleteModal" class="modal-overlay">
      <div class="modal-confirm">
        <h3>¿Eliminar Usuario?</h3>
        <p>Esta acción no se puede deshacer. ¿Estás seguro?</p>
        <div class="modal-actions">
          <button 
            @click="showDeleteModal = false" 
            class="btn-cancel" 
            :disabled="isDeleting"
          >
            Cancelar
          </button>
          
          <button 
            @click="executeDelete" 
            class="btn-danger" 
            :disabled="isDeleting"
          >
            {{ isDeleting ? 'Eliminando...' : 'Sí, eliminar' }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.main-container {
  max-width: 1100px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 20px;
  box-sizing: border-box;
  font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  color: #333;
}

.top-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
  border-bottom: 2px solid #f0f2f5;
  padding-bottom: 20px;
}

h1 {
  color: #004481;
  margin: 0;
  font-size: 1.8rem;
  font-weight: 700;
}

.btn-primary {
  background-color: #004481;
  color: white;
  border: none;
  padding: 10px 24px;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.95rem;
  transition: background-color 0.2s, transform 0.1s;
}

.btn-primary:hover {
  background-color: #002a5c;
}

.btn-primary:active {
  transform: translateY(1px);
}

.btn-primary:disabled {
  background-color: #cfd8dc;
  cursor: not-allowed;
}

.error-banner {
  background-color: #ffebee;
  color: #c62828;
  padding: 12px;
  border-radius: 6px;
  margin-bottom: 20px;
  border-left: 4px solid #c62828;
}

.table-responsive {
  width: 100%;
  overflow-x: auto;
  box-shadow: 0 4px 6px rgba(0,0,0,0.05);
  border-radius: 8px;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(5px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-confirm {
  background: white;
  padding: 30px;
  border-radius: 12px;
  width: 90%;
  max-width: 400px;
  text-align: center;
  box-shadow: 0 10px 25px rgba(0,0,0,0.1);
  animation: fadeIn 0.2s ease-out;
}

.modal-confirm h3 {
  margin-top: 0;
  color: #d32f2f;
}

.modal-actions {
  display: flex;
  gap: 15px;
  justify-content: center;
  margin-top: 25px;
}

.btn-cancel {
  padding: 8px 20px;
  background-color: #e0e0e0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 600;
}

.btn-cancel:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.btn-danger {
  padding: 8px 20px;
  background-color: #d32f2f;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 600;
  min-width: 120px;
}

.btn-danger:hover {
  background-color: #b71c1c;
}

.btn-danger:disabled {
  background-color: #e57373;
  cursor: wait;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (max-width: 768px) {
  .main-container {
    padding: 20px 15px;
  }

  .top-bar {
    flex-direction: column;
    gap: 15px;
    align-items: stretch;
    text-align: center;
  }

  h1 {
    font-size: 1.5rem;
    margin-bottom: 5px;
  }

  .btn-primary {
    width: 100%;
    padding: 14px;
    font-size: 1rem;
  }

  .modal-confirm {
    width: 90%;
    padding: 20px;
  }

  .modal-actions {
    flex-direction: column-reverse;
    gap: 10px;
  }

  .btn-cancel, .btn-danger {
    width: 100%;
    padding: 12px;
  }
}
</style>