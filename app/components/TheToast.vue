<!-- app/components/TheToast.vue -->
<script setup>
import { CheckCircle, AlertCircle, Info, X } from 'lucide-vue-next';
const { toasts } = useToast();
</script>

<template>
  <div class="fixed top-6 left-1/2 -translate-x-1/2 z-[300] flex flex-col gap-2 w-full max-w-[380px] px-4 pointer-events-none">
    <TransitionGroup 
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="transform -translate-y-2 opacity-0"
      enter-to-class="transform translate-y-0 opacity-100"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div 
        v-for="toast in toasts" 
        :key="toast.id"
        class="pointer-events-auto flex items-center justify-between gap-3 px-4 py-3 rounded-lg shadow-xl border-0"
        :class="{
          'bg-white text-gray-900': toast.type === 'success',
          'bg-red-600 text-white': toast.type === 'error',
          'bg-blue-600 text-white': toast.type === 'info'
        }"
      >
        <div class="flex items-center gap-3">
          <!-- 图标 -->
          <div class="flex-shrink-0">
            <CheckCircle v-if="toast.type === 'success'" class="size-5" />
            <AlertCircle v-else-if="toast.type === 'error'" class="size-5" />
            <Info v-else class="size-5" />
          </div>
          
          <!-- 文字内容 -->
          <span class="text-sm font-medium tracking-wide">
            {{ toast.message }}
          </span>
        </div>
        
        <!-- 关闭按钮 -->
        <button 
          @click="removeToast(toast.id)" 
          class="p-1 hover:bg-black/10 rounded transition-colors"
        >
          <X class="size-4 opacity-80" />
        </button>
      </div>
    </TransitionGroup>
  </div>
</template>