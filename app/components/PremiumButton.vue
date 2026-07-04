<template>
  <NuxtLink
    :to="to"
    :target="isExternal ? '_blank' : undefined"
    :class="[
      'inline-flex cursor-pointer items-center justify-center gap-2 px-6 py-3 rounded-full font-medium transition-all duration-300 active:scale-95 group',
      variantClasses[variant],
      sizeClasses[size]
    ]"
  >
    <!-- 图标插槽 -->
    <slot name="icon">
      

    </slot>

    <!-- 文字 -->
    <span class="relative">
      <slot />
    </span>

    <!-- 右侧箭头（可选） -->
    <ArrowRight   v-if="withArrow" class="size-4 opacity-70 transition-transform group-hover:translate-x-1" />
  </NuxtLink>
</template>

<script setup lang="ts">
import {
     ArrowRight,
} from 'lucide-vue-next'
interface Props {
  to?: string
  icon?: string
  withArrow?: boolean
  variant?: 'primary' | 'glass' | 'outline' | 'neon'
  size?: 'sm' | 'md' | 'lg'
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'primary',
  size: 'md',
  withArrow: false
})

const isExternal = computed(() => props.to?.startsWith('http'))

// 样式变体定义
const variantClasses = {
  // 1. 苹果风格纯白（极简高对比）
  primary: 'bg-white text-black hover:bg-zinc-200 shadow-[0_0_20px_rgba(255,255,255,0.3)]',
  
  // 2. 流动玻璃风格（Skal Ventures 核心风格）
 glass: 'liquid-glass text-white border border-white/10 hover:border-blue-500/50 hover:bg-blue-500/10 hover:shadow-[0_0_20px_-5px_rgba(59,130,246,0.4)] transition-all duration-500',
  
  // 3. 边框极简风格
  outline: 'bg-transparent text-zinc-400 border border-zinc-800 hover:border-zinc-500 hover:text-white',
  
  // 4. 霓虹光感风格（适合吸引眼球的 CTA）
  neon: 'bg-primary text-white animate-shine shadow-[0_0_15px_rgba(147,51,234,0.5)] hover:shadow-[0_0_25px_rgba(147,51,234,0.8)]'
}

const sizeClasses = {
  sm: 'text-sm',
  md: 'px-6 py-3 text-base',
  lg: 'px-10 py-4 text-lg'
}

</script>