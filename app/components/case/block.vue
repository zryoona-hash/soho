<template>
  <section   class="bg-black text-white py-[100px] font-sans">
    <div  class="container mx-auto px-6 max-w-[960px] ">
      
      <!-- 1. 顶部标题区域 (保持不变) -->
      <div  class="flex flex-col items-center text-center mb-[80px]">
        <div v-scroll-reveal="{ delay: 0.2,y: 80}" class="px-4 py-1 rounded-md border border-zinc-800 bg-[#0a0a0a] text-[12px] text-zinc-400 mb-6 uppercase tracking-wider">
         {{ siteConfig?.index?.case?.case?.tag || "Products Studies" }}
        </div>
        <h2 v-scroll-reveal="{ delay: 0.2,y: 80}"  class="text-[42px] md:text-[64px] font-light leading-[1.1] tracking-[-0.03em] mb-6">
          {{ siteConfig?.index?.case?.case?.mainHeading || "Case Studies" }}
        </h2>
        <p v-scroll-reveal="{ delay: 0.2,y: 80}"  class="text-zinc-400 text-lg md:text-xl font-extralight opacity-90">
         {{ siteConfig?.index?.case?.case?.subtitle  }}
        </p>
      </div>

      <!-- 2. 核心内容：带动画切换 -->
      <div class="min-h-[500px] relative">
        <Transition name="fade-slide" mode="out-in">
          <div :key="currentIndex" class="grid grid-cols-1 md:grid-cols-2 gap-[60px] md:gap-[100px] items-start">
            
            <!-- 左侧：产品大图 -->
            <div v-scroll-reveal="{ delay: 0.2,y: 80}" class="w-full aspect-square rounded-[24px] overflow-hidden border border-white/10 bg-zinc-900">
              <img 
                :src="currentCase.image" 
                class="w-full h-full object-cover"
                :alt="currentCase.title"
              />
            </div>

            <!-- 右侧：详情内容 -->
            <div v-scroll-reveal="{ delay: 0.2,y: 80}" class="pt-4 flex flex-col">
              <div class="flex items-center gap-2 mb-8">
                <!-- 还原 Logo 图标 -->
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M12 2L2 7L12 12L22 7L12 2Z" stroke="white" stroke-width="2" stroke-linejoin="round"/>
                  <path d="M2 17L12 22L22 17" stroke="white" stroke-width="2" stroke-linejoin="round"/>
                  <path d="M2 12L12 17L22 12" stroke="white" stroke-width="2" stroke-linejoin="round"/>
                </svg>
                <span class="text-xl font-bold tracking-tighter uppercase">{{ currentCase.brand }}</span>
              </div>

              <h3 class="text-[28px] md:text-[32px] font-light leading-tight mb-6 italic">
                  {{ currentCase.title }}
              </h3>

              <p class="text-zinc-400 text-base md:text-[17px] font-extralight  leading-relaxed mb-10">
                {{ currentCase.description }}
              </p>

              <div class="space-y-4">
                <h4 class="text-zinc-500 text-sm font-medium">Impact :</h4>
                <ul class="space-y-3">
                  <li v-for="point in currentCase.impact" :key="point" class="flex items-center gap-3">
                    <div class="size-1 rounded-full bg-white"></div>
                    <span class="text-zinc-200 text-[15px]">{{ point }}</span>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </Transition>
      </div>

      <!-- 3. 底部导航控制 (已改为点击切换) -->
      <div v-scroll-reveal="{ delay: 0.2,y: 80}" class="mt-[80px] flex items-center justify-center gap-8">
        <!-- 左箭头 -->
        <button @click="prevCase" class="text-zinc-500 hover:text-white transition-colors cursor-pointer p-2">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="19" y1="12" x2="5" y2="12"></line><polyline points="12 19 5 12 12 5"></polyline></svg>
        </button>

        <button @click="nextCase" class="text-zinc-500 hover:text-white transition-all uppercase tracking-[0.2em] text-[11px] font-bold cursor-pointer hover:scale-105">
          Click to Explore ({{ currentIndex + 1 }} / 3)
        </button>

        <!-- 右箭头 -->
        <button @click="nextCase" class="text-zinc-500 hover:text-white transition-colors cursor-pointer p-2">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="5" y1="12" x2="19" y2="12"></line><polyline points="12 5 19 12 12 19"></polyline></svg>
        </button>
      </div>

    </div>
  </section>
</template>

<script setup lang="ts">
const siteConfig = useAppConfig()
const cases = siteConfig?.index?.case?.product || []
const currentIndex = ref(0)



const currentCase = computed(() => cases[currentIndex.value])

const nextCase = () => {
  currentIndex.value = (currentIndex.value + 1) % cases.length
}

const prevCase = () => {
  currentIndex.value = (currentIndex.value - 1 + cases.length) % cases.length
}
</script>

<style scoped>
/* 切换时的淡入淡出和微移位动画 */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.4s ease;
}

.fade-slide-enter-from {
  opacity: 0;
  transform: translateX(20px);
}

.fade-slide-leave-to {
  opacity: 0;
  transform: translateX(-20px);
}
</style>