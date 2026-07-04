<template>
  <section class="bg-black text-white py-24 px-6 font-sans">
    <div class="container mx-auto max-w-[860px]">
      
      <!-- 1. 顶部标题区域 (保持原样) -->
      <div class="flex flex-col items-center text-center mb-16">
        <div v-scroll-reveal="{ delay: 0.2,y: 80}" v-if="siteConfig?.index?.faq?.tag" class="px-4 py-1 rounded-md border border-zinc-800 bg-[#0a0a0a] text-[12px] font-medium text-zinc-400 mb-8 uppercase tracking-widest">
          {{ siteConfig?.index?.faq?.tag }}
        </div>
        <h2 v-scroll-reveal="{ delay: 0.2,y: 80}" class="text-[40px] md:text-[60px] font-light leading-[1.1] tracking-[-0.04em] mb-6">
          {{ siteConfig?.index?.faq?.mainHeading }}
        </h2>
        <p v-scroll-reveal="{ delay: 0.2,y: 80}" class="text-zinc-400 text-lg font-extralight opacity-90 max-w-xl mx-auto">
          {{ siteConfig?.index?.faq?.subtitle }}
        </p>
      </div>

      <!-- 2. FAQ 列表 -->
      <div v-scroll-reveal="{ delay: 0.2,y: 80}" class="space-y-4">
        <div 
          v-for="(item, index) in siteConfig?.index?.faq?.faqs" 
          :key="index"
          class="relative transition-all duration-500 rounded-2xl border group cursor-pointer overflow-hidden"
          :class="[
            activeIndex === index 
            ? 'border-blue-500/40 bg-[#0c0c0c] shadow-[0_20px_50px_-20px_rgba(59,130,246,0.2)]' 
            : 'border-white/5 bg-[#050505] hover:border-white/10'
          ]"
          @click="toggle(index)"
        >
          <!-- 问题行 -->
          <div class="flex items-center justify-between p-6 select-none z-10 relative">
            <span 
              class="text-lg md:text-xl font-light transition-colors duration-500"
              :class="activeIndex === index ? 'text-white' : 'text-white group-hover:text-zinc-200'"
            >
              {{ item.question }}
            </span>
            
            <!-- 箭头：增加发光感切换 -->
            <div class="flex-shrink-0 ml-4 size-8 flex items-center justify-center rounded-full border border-white/5 transition-all duration-500"
                 :class="{ 'rotate-180 bg-blue-600 border-blue-500 shadow-[0_0_15px_rgba(59,130,246,0.5)]': activeIndex === index }">
              <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" 
                   :class="activeIndex === index ? 'text-white' : 'text-zinc-500'">
                <path d="m6 9 6 6 6-6"/>
              </svg>
            </div>
          </div>

          <!-- 核心动画区域：使用 Grid 轨道高度过渡 -->
          <div 
            class="grid transition-[grid-template-rows,opacity] duration-500 ease-[cubic-bezier(0.4,0,0.2,1)]"
            :class="[activeIndex === index ? 'grid-rows-[1fr] opacity-100' : 'grid-rows-[0fr] opacity-0']"
          >
            <div class="overflow-hidden">
              <div class="px-6 pb-6 text-zinc-500 text-base md:text-lg font-light leading-relaxed">
                <!-- 分割线也带一个淡入效果 -->
                <div class="pt-4 border-t border-white/5 transform transition-transform duration-700"
                     :class="[activeIndex === index ? 'translate-y-0' : '-translate-y-2']">
                  {{ item.answer }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>
<script setup lang="ts">
// 1. 获取全局配置
const siteConfig = useAppConfig()


// 2. 状态控制 (修正：只声明一次)
const activeIndex = ref<number | null>(null) 

const toggle = (index: number) => {
  activeIndex.value = activeIndex.value === index ? null : index
}
</script>

<style scoped>
h2 {
  text-wrap: balance;
}

/* 蓝色投影的增强效果，确保在深色背景下有柔和的边缘感 */
.border-blue-500 {
  border-color: #3b82f6 !important;
}

/* 隐藏滚动条 */
.overflow-hidden {
  will-change: max-height, opacity;
}
</style>