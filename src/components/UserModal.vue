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
const isSaving = ref(false) 

watch(() => props.show, (isOpen) => {
  if (isOpen) {
    error.value = ''
    isSaving.value = false 
    if (props.userData) {
      form.value = { ...props.userData }
    } else {
      form.value = { id: null, name: '', username: '', email: '', phone: '' }
    }
  }
})

const handlePhoneInput = (event) => {
  form.value.phone = event.target.value.replace(/\D/g, '')
}

const isValidEmail = (email) => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
}

const handleSubmit = () => {
  
  if (!form.value.name.trim() || 
      !form.value.username.trim() || 
      !form.value.email.trim() || 
      !form.value.phone.trim()) {
    error.value = 'Todos los campos son obligatorios.'
    return
  }

  if (!isValidEmail(form.value.email)) {
    error.value = 'El formato del correo no es válido (ej: usuario@dominio.com).'
    return
  }

  
  isSaving.value = true

  
  setTimeout(() => {
    emit('save', { ...form.value })
    isSaving.value = false 
  }, 1500)
}
</script>

<template>
  <div v-if="show" class="modal-overlay">
    <div class="modal-content">
      <div class="modal-header">
        <h2>{{ form.id ? 'Editar Usuario' : 'Nuevo Usuario' }}</h2>
      </div>
      
      <form @submit.prevent="handleSubmit" novalidate>
        
        <div class="form-group">
          <label>Nombre completo</label>
          <input 
            v-model="form.name" 
            type="text" 
            placeholder="Ej: Juan Pérez" 
            maxlength="50"
            :disabled="isSaving"
          />
        </div>
        
        <div class="form-group">
          <label>Nombre de usuario</label>
          <input 
            v-model="form.username" 
            type="text" 
            placeholder="Ej: jperez" 
            maxlength="20"
            :disabled="isSaving"
          />
        </div>
        
        <div class="form-group">
          <label>Correo electrónico</label>
          <input 
            v-model="form.email" 
            type="email" 
            placeholder="juan@ejemplo.com" 
            maxlength="50"
            :disabled="isSaving"
          />
        </div>
        
        <div class="form-group">
          <label>Teléfono (Solo números)</label>
          <input 
            v-model="form.phone"
            @input="handlePhoneInput"
            type="text" 
            placeholder="Ej: 999123456" 
            maxlength="15"
            :disabled="isSaving"
          />
        </div>

        <div v-if="error" class="error-msg">
          {{ error }}
        </div>

        <div class="modal-actions">
          <button 
            type="button" 
            @click="emit('close')" 
            class="btn-cancel"
            :disabled="isSaving"
          >
            Cancelar
          </button>
          
          <button 
            type="submit" 
            class="btn-save"
            :disabled="isSaving"
          >
            {{ isSaving ? 'Guardando...' : 'Guardar' }}
          </button>
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
  backdrop-filter: blur(5px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  padding: 20px;
  box-sizing: border-box;
}

.modal-content {
  background: white;
  padding: 30px;
  border-radius: 12px;
  width: 100%;
  max-width: 450px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.1);
  animation: fadeIn 0.2s ease-out;
}

.modal-header h2 {
  margin: 0 0 25px 0;
  color: #004481;
  font-size: 1.5rem;
  border-bottom: 1px solid #eee;
  padding-bottom: 15px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 600;
  color: #444;
  font-size: 0.9rem;
}

.form-group input {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 1rem;
  box-sizing: border-box;
  transition: border-color 0.2s, box-shadow 0.2s;
}

.form-group input:focus {
  border-color: #004481;
  outline: none;
  box-shadow: 0 0 0 3px rgba(0, 68, 129, 0.1);
}

.form-group input:disabled {
  background-color: #f9f9f9;
  cursor: not-allowed;
}

.error-msg {
  color: #d32f2f;
  font-size: 0.9rem;
  margin-bottom: 20px;
  padding: 10px;
  background-color: #ffebee;
  border-radius: 6px;
  border-left: 4px solid #d32f2f;
  text-align: center;
  font-weight: 500;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  margin-top: 30px;
}

button {
  padding: 10px 24px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.95rem;
  transition: background-color 0.2s;
}

.btn-cancel {
  background-color: #f1f3f5;
  color: #495057;
}

.btn-cancel:hover { background-color: #e9ecef; }
.btn-cancel:disabled { opacity: 0.6; cursor: not-allowed; }

.btn-save {
  background-color: #004481;
  color: white;
  min-width: 120px; 
}

.btn-save:hover { background-color: #002a5c; }
.btn-save:disabled { 
  background-color: #7fa8ce; 
  cursor: wait; 
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>