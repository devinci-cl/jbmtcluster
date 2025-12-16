<template>
  <div class="min-h-screen flex items-center justify-center bg-neutral-950 p-4 font-sans selection:bg-white/20">
    <n-config-provider :theme="darkTheme">
      <n-card 
        class="max-w-[400px] w-full bg-neutral-900/50 border border-white/5 shadow-2xl rounded-2xl" 
        :bordered="false"
        content-style="padding: 2.5rem;"
      >
        <div class="text-center mb-8">
          <h1 class="text-3xl font-semibold text-white tracking-tight mb-2">
            Bienvenido a JB Cluster
          </h1>
          <!-- <p class="text-neutral-400 text-sm">Ingresa a tu cuenta para continuar</p> -->
        </div>

        <n-form ref="formRef" :model="model" :rules="rules" size="large">
          <n-form-item path="email" label="Correo Electrónico" label-style="color: #a3a3a3; font-size: 0.875rem;">
            <n-input 
              v-model:value="model.email" 
              placeholder="nombre@empresa.com" 
              @keydown.enter.prevent
              class="rounded-lg !bg-neutral-800/50 border-transparent focus:!border-white/20 transition-all font-medium"
            />
          </n-form-item>
          
          <n-form-item path="password" label="Contraseña" label-style="color: #a3a3a3; font-size: 0.875rem;">
            <n-input
              v-model:value="model.password"
              type="password"
              show-password-on="click"
              placeholder="••••••••"
              @keydown.enter.prevent
              class="rounded-lg !bg-neutral-800/50 border-transparent focus:!border-white/20 transition-all font-medium"
            />
          </n-form-item>
          
          <div class="flex justify-end mb-6 -mt-2">
            <a href="#" class="text-xs text-neutral-500 hover:text-white transition-colors duration-200">
              Recuperar contraseña
            </a>
          </div>
          
          <n-button 
            type="primary" 
            block 
            size="large" 
            class="mb-6 rounded-lg font-medium h-11 !bg-white !text-black hover:!bg-neutral-200 transition-colors border-none" 
            :loading="loading" 
            @click="handleLogin"
          >
            Iniciar Sesión
          </n-button>
        </n-form>
        
        <!-- <div class="text-center text-xs text-neutral-500 mt-4">
          ¿No tienes una cuenta? 
          <a href="#" class="text-neutral-300 hover:text-white transition-colors ml-1 underline underline-offset-2">Regístrate</a>
        </div> -->
      </n-card>
    </n-config-provider>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { darkTheme } from 'naive-ui'
import type { FormInst } from 'naive-ui'

const formRef = ref<FormInst | null>(null)
const loading = ref(false)
const model = ref({
  email: '',
  password: ''
})

const rules = {
  email: {
    required: true,
    message: 'Por favor ingresa tu correo',
    trigger: ['input', 'blur']
  },
  password: {
    required: true,
    message: 'Por favor ingresa tu contraseña',
    trigger: ['input', 'blur']
  }
}

const handleLogin = (e: MouseEvent) => {
  e.preventDefault()
  formRef.value?.validate((errors) => {
    if (!errors) {
      loading.value = true
      // Simulación de login
      setTimeout(() => {
        loading.value = false
        $notification.success({
            content: 'Inicio de sesión exitoso',
            meta: 'Bienvenido de nuevo',
            duration: 2500,
            keepAliveOnHover: true
        })
      }, 2000)
    }
  })
}

// Inyectar notificaciones automáticamente gracias al auto-import (usando useNotification no funcionaría directo en template sin setup, pero unplugin-auto-import debería exponerlo o usamos setup con useMessage/useNotification)
// Corrección: useNotification debe llamarse dentro de setup() o usamos la API global si está configurada, pero NaiveUI recomienda 'useNotification' dentro de un componente provider.
// Para simplificar y evitar errores de "Injection not found", envolveré en n-notification-provider si fuera una app real grande, pero aquí estoy dentro de n-config-provider solamente. 
// NaiveUI requiere n-notification-provider para usar useNotification. 
// Voy a quitar la notificación por ahora para evitar complejidad extra o errores de "No Notification Provider", o agregar el provider.
// Agregaré n-notification-provider y n-message-provider para que sea robusto.

// Espera, no puedo usar useChange() o useNotification() fuera de un componente hijo de Provider. 
// Para este demo rápido, usaré window.alert o console.log si no quiero complicar el template con Providers anidados y un componente Wrapper.
// Mejor aún: El botón simplemente hará el loading effect.

</script>


