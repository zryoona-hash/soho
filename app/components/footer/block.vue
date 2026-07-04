<template>
  <footer class="relative bg-black overflow-hidden border-t border-white/5">
    <!-- 1. 顶部蓝色氛围光 -->
    <div class="absolute top-0 left-1/2 -translate-x-1/2 w-[1000px] h-[300px] bg-blue-600/10 blur-[120px] rounded-full -translate-y-1/2 pointer-events-none"></div>

    <div class=" mx-auto px-6 max-w-[1200px]  border-white/5 pt-24 pb-12 relative z-10">
      <!-- 核心内容区：修正为标准 12 栅格 -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-12 gap-12 lg:gap-4 mb-24">

        <!-- A. 品牌与订阅区 (占 5 列) -->
        <div class="lg:col-span-5 flex flex-col items-center lg:items-start text-center lg:text-left">
          <NuxtLink to="/" class="flex items-center gap-3 mb-6 group">
            <div class="flex size-10 items-center justify-center">
              <img v-if="siteConfig?.logo?.logoUrl" :src="siteConfig?.logo?.logoUrl" alt="Logo" class="w-full h-full object-contain">
            </div>
          
          </NuxtLink>

          <p class="text-zinc-400 text-base leading-relaxed max-w-sm font-light mb-10">
            {{ siteConfig?.logo?.slogan  }} 
          </p>

          <!-- 订阅邮件 -->
          <CatalogFetcher />
        </div>

        <!-- B. 占位列 (占 1 列，增加呼吸感) -->
        <div class="hidden lg:block lg:col-span-1"></div>

        <!-- C. 导航链接 (2+2+2 = 6 列) -->
        

        <div class="lg:col-span-3 text-center lg:text-right">
          <h3 class="text-[11px] font-black text-zinc-500 uppercase tracking-[0.25em] mb-8">Pages</h3>
          <ul class="space-y-4">
            <li v-for="page in pages" :key="page.label">
              <NuxtLink :to="page.href" class="text-zinc-400 hover:text-white transition-colors font-light text-sm">{{ page.label }}</NuxtLink>
            </li>
          </ul>
        </div>

        <div class="lg:col-span-3 text-center  lg:text-right">
          <h3 class="text-[11px] font-black text-zinc-500 uppercase tracking-[0.25em] mb-8">Socials</h3>
          <ul class="space-y-4">
            <li v-for="social in siteConfig?.footer?.socials" :key="social.label">
              <a :href="social.href" target="_blank" class="text-zinc-400 hover:text-white transition-colors font-light text-sm">{{ social.label }}</a>
            </li>
          </ul>
        </div>
      </div>

      <!-- 底部版权区 -->
      <div class="pt-10 border-t border-white/5 flex flex-col md:flex-row justify-between items-center gap-6">
        <div class="text-zinc-600 text-[12px] tracking-wide font-light">
          &copy; {{ new Date().getFullYear() }} {{ siteConfig?.base?.companyName || 'DaddyTemplate' }}. All rights reserved.
        </div>

        <div class="flex items-center gap-8 text-[10px] font-bold uppercase tracking-[0.2em]">
          <a href="https://www.daddytemplate.com" target="_blank" class="text-zinc-700 hover:text-primary transition-colors flex items-center gap-1">
            Powered by <span class="text-zinc-500 font-black">DaddyTemplate</span>
          </a>
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
const siteConfig = useAppConfig()



const pages = [
    { label: 'Home', href: '/' },
    { label: 'About', href: '/about' },
    { label: 'Products', href: '/products' }
]


</script>