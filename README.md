# Gestión de Usuarios - Prueba Técnica Frontend

Aplicación web desarrollada con **Vue 3 (Composition API)** que implementa un sistema CRUD completo para la gestión de usuarios.

## 📋 Descripción
Este proyecto cumple con los requerimientos de la prueba técnica, consumiendo datos iniciales de una API pública y gestionando las operaciones de creación, edición y eliminación mediante estado local (in-memory), sin persistencia externa según lo solicitado.

## 🚀 Tecnologías Utilizadas
* **Vue.js 3** (Composition API con `<script setup>`)
* **Vite** (Entorno de desarrollo)
* **Node.js** (v22.11.0)
* **Axios** (Consumo de API)
* **CSS 3** (Estilos personalizados y diseño Responsive)

## ✨ Funcionalidades
* **Listado de Usuarios:** Obtención de datos desde JSONPlaceholder con indicador de carga (*loading*).
* **CRUD Local:**
    * **Crear:** Formulario en modal con validaciones y generación de ID secuencial.
    * **Editar:** Precarga de datos y actualización en tiempo real.
    * **Eliminar:** Confirmación de seguridad y actualización inmediata del listado.
* **Validaciones:** Verificación de campos obligatorios y formato de email.
* **Responsive:** Tabla adaptable para visualización correcta en dispositivos móviles.

## 🛠️ Instalación y Ejecución

Sigue estos pasos para correr el proyecto localmente:

1.  **Clonar el repositorio:**
    ```bash
    git clone [https://github.com/JosephMTC/vue3-crud-users.git](https://github.com/JosephMTC/vue3-crud-users.git)
    ```

2.  **Entrar a la carpeta e instalar dependencias:**
    ```bash
    cd vue3-crud-users
    npm install
    ```

3.  **Iniciar el servidor:**
    ```bash
    npm run dev
    ```
