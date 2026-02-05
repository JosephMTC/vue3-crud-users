<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  show: Boolean,
  userData: Object
})

const emit = defineEmits(['close', 'save'])

const form = ref({
  id: null,
  name: '',
  username: '',
  email: '',
  phone: ''
})

const error = ref('')

watch(() => props.show, (isOpen) => {
  if (isOpen) {
    if (props.userData) {
   
      form.value = { ...props.userData }
    } else {
     
      form.value = { id: null, name: '', username: '', email: '', phone: '' }
    }
    error.value = ''
  }
})

const isValidEmail = (email) => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
}

const handleSubmit = () => {
  if (!form.value.name || !form.value.username || !form.value.email || !form.value.phone) {
    error.value = 'Todos los campos son obligatorios'
    return
  }

  if (!isValidEmail(form.value.email)) {
    error.value = 'El formato del correo no es válido'
    return
  }

  emit('save', { ...form.value })
}
</script>

<template>
  <div v-if="show" class="modal-overlay">
    <div class="modal-content">
      <div class="modal-header">
        <h2>{{ form.id ? 'Editar Usuario' : 'Nuevo Usuario' }}</h2>
      </div>
      
      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label>Nombre completo</label>
          <input v-model="form.name" type="text" placeholder="Ej: Juan Pérez" />
        </div>
        
        <div class="form-group">
          <label>Nombre de usuario</label>
          <input v-model="form.username" type="text" placeholder="Ej: jperez" />
        </div>
        
        <div class="form-group">
          <label>Correo electrónico</label>
          <input v-model="form.email" type="email" placeholder="juan@ejemplo.com" />
        </div>
        
        <div class="form-group">
          <label>Teléfono</label>
          <input v-model="form.phone" type="text" placeholder="Ej: 555-1234" />
        </div>

        <div v-if="error" class="error-msg">
          {{ error }}
        </div>

        <div class="modal-actions">
          <button type="button" @click="emit('close')" class="btn-cancel">Cancelar</button>
          <button type="submit" class="btn-save">Guardar</button>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  padding: 20px;
  box-sizing: border-box;
}

.modal-content {
  background: white;
  padding: 24px;
  border-radius: 8px;
  width: 100%;
  max-width: 450px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.modal-header h2 {
  margin: 0 0 20px 0;
  color: #2c3e50;
}

.form-group {
  margin-bottom: 16px;
}

.form-group label {
  display: block;
  margin-bottom: 6px;
  font-weight: 500;
  color: #555;
}

.form-group input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  box-sizing: border-box;
  transition: border-color 0.2s;
}

.form-group input:focus {
  border-color: #3498db;
  outline: none;
}

.error-msg {
  color: #e74c3c;
  font-size: 0.9rem;
  margin-bottom: 16px;
  padding: 8px;
  background-color: #fdeaea;
  border-radius: 4px;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  margin-top: 24px;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.95rem;
}

.btn-cancel {
  background-color: #f1f2f6;
  color: #7f8c8d;
}

.btn-save {
  background-color: #2ecc71;
  color: white;
}

.btn-cancel:hover { background-color: #e5e7eb; }
.btn-save:hover { background-color: #27ae60; }
</style>