<template>
  <section class="bg-black text-white py-24 px-6 font-sans">
    <div class="container mx-auto max-w-[960px]">
      
      <!-- 1. 头部区域 -->
      <div class="flex flex-col items-center text-center mb-20">
        <!-- 胶囊标签 -->
        <div v-scroll-reveal class="px-4 py-1.5 rounded-lg border border-zinc-800 bg-zinc-900/50 text-[12px] font-medium text-zinc-400 mb-8 uppercase tracking-widest">
          {{ siteConfig?.index?.benefitsGrid?.tag }}
        </div>
        <!-- 大标题 -->
        <h2 v-scroll-reveal class="text-[42px] md:text-[64px] font-light leading-[1.1] tracking-[-0.03em] mb-6">
          {{ siteConfig?.index?.benefitsGrid?.mainHeading }}
        </h2>
        <!-- 描述文本 -->
        <p v-scroll-reveal class="text-zinc-400 text-lg md:text-xl font-extralight opacity-90">
          {{ siteConfig?.index?.benefitsGrid?.subtitle }}
        </p>
      </div>

      <!-- 2. 核心 3x2 网格 -->
      <div v-scroll-reveal class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div 
        
          v-for="benefit in siteConfig?.index?.benefitsGrid?.benefits" 
          :key="benefit.title"
          class="relative group overflow-hidden rounded-2xl border border-white/10 bg-[#050505] p-10 flex flex-col h-full transition-all duration-500 hover:border-blue-500/30"
        >
          <!-- 底部蓝色渐变光晕 (高度还原图中的柔和过渡) -->
          <div class="absolute -bottom-1 left-0 right-0 h-32 bg-gradient-to-t from-blue-600/15 via-blue-600/5 to-transparent pointer-events-none transition-opacity duration-500 group-hover:from-blue-600/25"></div>

          <!-- 图标 -->
          <div class="mb-8 flex items-center justify-start">
            <component :is="iconMap[benefit.icon]" class="size-8 text-white" />
          </div>

          <!-- 内容 -->
          <div class="relative z-10 flex flex-col h-full">
            <h3 class="text-2xl font-bold mb-4 tracking-tight text-white/95">
              {{ benefit.title }}
            </h3>
            <p class="text-zinc-500 text-[16px] leading-relaxed font-normal">
              {{ benefit.description }}
            </p>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<script setup lang="ts">
const siteConfig = useAppConfig()
import { 
  Zap, 
  Users, 
  Clock, 
  DollarSign, 
  Database, 
  TrendingUp 
} from 'lucide-vue-next'
// 2. 建立一个局部映射表
const iconMap = {
  Zap,
  Users,
  Clock,
  DollarSign,
  Database,
  TrendingUp
}
</script>

<style scoped>
/* 确保标题在宽屏下的对比度 */
h2 {
  text-wrap: balance;
}

/* 模拟图片中的细微质感 */
.rounded-2xl {
  box-shadow: 0 4px 24px -1px rgba(0, 0, 0, 0.2);
}
</style>