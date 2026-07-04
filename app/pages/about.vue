<template>
  <main class="bg-black text-white font-sans overflow-hidden">

    <!-- 1. Hero Section: 品牌宣言 -->
    <section class="relative pt-32 pb-20 px-6">
      <!-- 背景装饰：巨大的模糊光晕 -->
      <div
        class="absolute top-0 left-1/2 -translate-x-1/2 w-full max-w-[1600px] h-[400px] bg-blue-600/10 blur-[120px] pointer-events-none">
      </div>

      <div class="container mx-auto max-w-[960px] text-center relative z-10">
        <div v-scroll-reveal
          class="inline-block px-4 py-1.5 rounded-lg border border-zinc-800 bg-zinc-900/50 text-[12px] font-medium text-zinc-400 mb-8 uppercase tracking-widest">

          {{ siteConfig?.about?.tag || 'WHO WE ARE' }}
        </div>

        <!-- 使用你最喜欢的 header-reveal 效果 -->
        <h1 v-scroll-reveal="{ delay: 0.4 }" class="text-[42px] md:text-[64px] font-light leading-[1.1] tracking-[-0.03em] mb-6">
         {{ siteConfig?.about?.titleOne || 'We bridge the gap between ' }} <span class="text-blue-500">{{ siteConfig?.about?.titleTwo || 'vision' }}</span> {{ siteConfig?.about?.titleThree || 'and reality.' }}
        </h1>

        <p v-scroll-reveal="{ delay: 0.4 } "
        v-if="siteConfig?.about?.description"
          class="text-zinc-400 text-lg md:text-xl font-extralight max-w-[700px] mx-auto leading-relaxed">
          {{ siteConfig?.about?.description || '老爹模板为外贸企业提供免费模板,帮助他们快速启动业务,提高效率。' }}
        </p>
      </div>
    </section>

    <!-- 2. Stats Section: 数据证明 -->
   <section class="py-24 border-y border-white/5 bg-[#020202] overflow-hidden">
    <div class="container mx-auto max-w-[1100px] px-6">
      
      <!-- 4列布局：手机端 2x2，桌面端 1x4 -->
      <div class="grid grid-cols-2 md:grid-cols-4 gap-y-12 md:gap-y-0">
        
        <div 
          v-for="(stat, index) in siteConfig?.about?.stats" 
          :key="index" 
          v-scroll-reveal="{ delay: index * 0.1, y: 20 }" 
          class="relative text-center group"
        >
          <!-- 装饰：每个数字背后的极弱光晕 -->
          <div class="absolute inset-0 bg-blue-600/5 blur-[40px] rounded-full opacity-0 group-hover:opacity-100 transition-opacity duration-700"></div>

          <!-- 桌面端的分隔线 (最后一个不加) -->
          <div v-if="index !== siteConfig.about.stats.length - 1" 
               class="hidden md:block absolute right-0 top-1/2 -translate-y-1/2 w-[1px] h-12 bg-gradient-to-b from-transparent via-white/10 to-transparent">
          </div>

          <!-- 数字：增加字重的层次感 -->
          <div class="relative">
            <span v-count class="text-[48px] md:text-[64px] font-bold tracking-tighter text-white inline-block">
              {{ stat.number }}
            </span>
            <!-- 蓝点装饰：让数字更有设计感 -->
            <span class="text-blue-500 ml-0.5">.</span>
          </div>

          <!-- 标签：增加字母间距和字重 -->
          <div class="text-zinc-500 text-[10px] md:text-xs uppercase tracking-[0.2em] font-bold mt-2">
            {{ stat.label }}
          </div>

        </div>
      </div>
    </div>
  </section>
    <!-- 3. Story Section: 故事/愿景 -->
    <section class="py-24 px-6">
      <div class="container mx-auto max-w-[1100px]">
        <!-- 外层容器：使用 min-h 确保高度随内容自动拉伸 -->
        <div class="relative rounded-[3rem] overflow-hidden min-h-fit">

          <!-- 背景装饰 -->
          <div class="absolute -top-24 -left-24 size-96 bg-blue-600/10 blur-[100px] pointer-events-none"></div>

          <div class="relative max-w-[960px] py-4 mx-auto z-10 grid grid-cols-1 lg:grid-cols-12 gap-16 items-start">

            <!-- 左侧：文字介绍（高度自适应） -->
            <div class="lg:col-span-6 flex flex-col h-full lg:max-h-[600px]"> <!-- 限制整体最大高度 -->

              <!-- 1. 顶部标签 (固定，不随内容滚动) -->
              <div v-scroll-reveal
                class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-blue-500/10 border border-blue-500/20 text-blue-400 text-[10px] font-bold uppercase tracking-widest mb-6 w-fit">
                <span class="relative flex h-2 w-2">
                  <span
                    class="animate-ping absolute inline-flex h-full w-full rounded-full bg-blue-400 opacity-75"></span>
                  <span class="relative inline-flex rounded-full h-2 w-2 bg-blue-500"></span>
                </span>
                Company Story
              </div>

              <!-- 2. 标题 (固定，不随内容滚动) -->
              <h2 v-scroll-reveal="{ delay: 0.1 }"
                class="text-[42px]  w-full text-left md:text-[48px] font-light leading-[1.1] tracking-[-0.03em]">
                {{ siteConfig?.about?.storyTitle || 'Crafting the Future.' }}
              </h2>

              <!-- 3. 可滚动文字区域 (核心修改点) -->
              <!-- 我们给它一个固定高度，并开启自定义滚动条样式 -->
              <div v-scroll-reveal="{ delay: 0.2 }" class="flex-1 overflow-y-auto pr-4 custom-scrollbar relative"
                style="max-height: 350px;">
                <!-- 渐变遮罩：给底部加一个淡出效果，提示下方还有内容 -->
                <div
                  class="sticky top-0 h-4 w-full bg-gradient-to-b from-[#050505] to-transparent z-10 pointer-events-none">
                </div>

                <div class="space-y-6 text-zinc-400 text-base md:text-lg font-light leading-relaxed pb-8">
                  <p v-for="(p, i) in siteConfig?.about?.storyParagraphs" :key="i">
                    {{ p }}
                  </p>
                </div>

                <div
                  class="sticky bottom-0 h-12 w-full bg-gradient-to-t from-[#050505] via-[#050505]/80 to-transparent z-10 pointer-events-none">
                </div>
              </div>


            </div>

            <!-- 右侧：多图切换轮播 (占 6 列) -->
            <div class="lg:col-span-6 w-full">
              <div v-scroll-reveal="{ x: 40, delay: 0.4 }"
                class="relative group aspect-[4/5] md:aspect-square lg:aspect-[4/5] rounded-[2.5rem] overflow-hidden border border-white/10 bg-white/5 shadow-2xl">

                <!-- 图片显示层 (带切换动画) -->
                <div class="absolute inset-0">
                  <Transition name="fade">
                    <img :key="currentImgIndex" :src="siteConfig?.about?.gallery[currentImgIndex]" class="w-full h-full object-cover"
                      alt="Gallery Image" />
                  </Transition>
                </div>

                <!-- 图片覆盖层：底部阴影 -->
                <div
                  class="absolute inset-0 bg-gradient-to-t from-black/80 via-transparent to-transparent pointer-events-none">
                </div>

                <!-- 切换按钮组 -->
                <div class="absolute bottom-8 right-8 flex items-center gap-3">
                  <button @click="prevImg"
                    class="size-12 flex items-center justify-center rounded-full bg-black/40 backdrop-blur-md border border-white/10 text-white hover:bg-white hover:text-black transition-all duration-300">
                    <ChevronLeft class="size-6" />
                  </button>
                  <button @click="nextImg"
                    class="size-12 flex items-center justify-center rounded-full bg-black/40 backdrop-blur-md border border-white/10 text-white hover:bg-white hover:text-black transition-all duration-300">
                    <ChevronRight class="size-6" />
                  </button>
                </div>

                <!-- 图片计数器 -->
                <div class="absolute bottom-10 left-10">
                  <div class="flex items-center gap-2">
                    <span v-for="(_, i) in siteConfig?.about?.gallery" :key="i"
                      :class="i === currentImgIndex ? 'w-8 bg-blue-500' : 'w-1.5 bg-white/30'"
                      class="h-1.5 rounded-full transition-all duration-500">
                    </span>
                  </div>
                </div>

              </div>
            </div>

          </div>
        </div>
      </div>
    </section>
    <!-- 4. Core Values: 核心价值观 (复用你的网格风格) -->
    <section class="bg-black text-white py-24 px-6 font-sans">
    <div class="container mx-auto max-w-[960px]">
      
      <!-- 1. 头部区域 -->
      <div class="flex flex-col items-center text-center mb-20">
        <!-- 胶囊标签 -->
        <div v-scroll-reveal="{ x: 40, delay: 0.4 }" class="px-4 py-1.5 rounded-lg border border-zinc-800 bg-zinc-900/50 text-[12px] font-medium text-zinc-400 mb-8 uppercase tracking-widest">
          {{ siteConfig?.about?.ourvalue?.tag }}
        </div>
        <!-- 大标题 -->
        <h2 v-scroll-reveal="{ x: 40, delay: 0.4 }" class="text-[42px]  w-full text-left md:text-[48px] font-light leading-[1.1] tracking-[-0.03em] mb-6">
          {{ siteConfig?.about?.ourvalue?.title }}
        </h2>
        <!-- 描述文本 -->
        <p v-scroll-reveal="{ x: 40, delay: 0.4 }" class="text-zinc-400  text-left w-full text-lg md:text-xl font-extralight opacity-90">
          {{ siteConfig?.about?.ourvalue?.subtitle }}
        </p>
      </div>

      <!-- 2. 核心 3x2 网格 -->
      <div v-scroll-reveal="{ x: 40, delay: 0.4 }" class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div 
        
          v-for="benefit in siteConfig?.about?.ourvalue?.benefits" 
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



  </main>
</template>

<script setup lang="ts">
const siteConfig = useAppConfig()
import { 
  Users, 
  Clock, 
  DollarSign, 
  Database, 
  TrendingUp,
  Zap,
  Target,
  ShieldCheck,
  Cpu,
  ArrowRight,
  Globe,
  Sparkles,
  ChevronLeft, ChevronRight
} from 'lucide-vue-next'




const currentImgIndex = ref(0)

const nextImg = () => {
  currentImgIndex.value = (currentImgIndex.value + 1) % siteConfig?.about?.gallery.length
}

const prevImg = () => {
  currentImgIndex.value = (currentImgIndex.value - 1 + siteConfig?.about?.gallery.length) % siteConfig?.about?.gallery.length
}

const iconMap = {
  Zap,
  Users,
  Clock,
  DollarSign,
  Database,
  TrendingUp,
  Target,
  ShieldCheck,
  Cpu,
  ArrowRight,
  Globe,
  Sparkles,
  ChevronLeft, ChevronRight
}


const values = [
  {
    icon: 'Target',
    title: 'Precision First',
    desc: 'We don\'t believe in "close enough." Every detail is intentional, every pixel is deliberate.'
  },
  {
    icon: 'Sparkles',
    title: 'Innovation Driven',
    desc: 'Constantly exploring the frontier of technology to bring you tomorrow\'s solutions today.'
  },
  {
    icon: 'ShieldCheck',
    title: 'Unwavering Quality',
    desc: 'Reliability is our foundation. We build robust systems that stand the test of time and scale.'
  }
]
useSeoMeta({
  title: () => siteConfig?.seo?.about.title || 'Default Title',
  description: () => siteConfig?.seo?.about.description || 'Default description',
  ogTitle: () => siteConfig?.seo?.about.title || 'Default Title',
  ogDescription: () => siteConfig?.seo?.about.description || 'Default description',
  ogImage: () => siteConfig?.seo?.about.ogImage || '/default-og.jpg',
  twitterCard: () => siteConfig?.seo?.about.twitterCard || 'summary_large_image',
})
</script>

<style scoped>
/* 标题文字平衡 */
h1,
h2 {
  text-wrap: balance;
}

/* 文字渐变效果 */
.text-italic {
  font-style: italic;
  font-family: serif;
  /* 增加一点衬线体斜体，更有设计感 */
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.8s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
