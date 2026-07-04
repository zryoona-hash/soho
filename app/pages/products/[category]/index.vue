<!-- pages/products/[category]/index.vue -->
<template>
  <div class="bg-black min-h-screen pt-32 pb-24 text-white font-sans overflow-hidden">
    <!-- 背景装饰：顶部柔和蓝光 -->
    <div
      class="absolute top-0 left-1/2 -translate-x-1/2 w-full max-w-[1200px] h-[300px] bg-blue-600/5 blur-[120px] pointer-events-none">
    </div>

    <div class="container mx-auto px-6 max-w-[1300px] relative z-10">

      <!-- 1. 面包屑 (Breadcrumbs) - 暗黑极简版 -->
      <nav v-scroll-reveal
        class="mb-12 flex items-center gap-2 text-[10px] uppercase tracking-[0.2em] font-bold text-zinc-500">
        <NuxtLink to="/" class="hover:text-blue-500 transition-colors">Home</NuxtLink>
        <span class="opacity-30">/</span>
        <NuxtLink to="/products" class="hover:text-blue-500 transition-colors">Products</NuxtLink>
        <span class="opacity-30">/</span>
        <span class="text-zinc-300 capitalize">{{ categoryName }}</span>
      </nav>

      <!-- 2. 标题区域 - 采用 header-reveal 动效 -->
      <div class="mb-16 max-w-[800px]">
        <div v-scroll-reveal
          class="inline-block px-3 py-1 rounded-full bg-blue-500/10 border border-blue-500/20 text-blue-400 text-[10px] font-bold uppercase tracking-widest mb-6">
          Collections
        </div>
        <h1 v-scroll-reveal
          class="text-[42px] md:text-[60px] font-bold text-white tracking-[-0.04em] leading-[1.1] mb-6 capitalize">
          See Our Collections <span class="text-blue-500">{{ categoryName }}</span>
        </h1>

      </div>

      <!-- 3. 产品网格 - 保持逻辑，注入动画 -->
      <div v-if="products && products.length > 0"
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 2xl:grid-cols-4 gap-8">
        <div v-for="(item, index) in products" :key="item._path" v-scroll-reveal="{ delay: index * 0.1, y: 20 }">
          <ProductCard :product="item" />
        </div>
      </div>

      <!-- 4. 空状态 (Empty State) - 改造成暗黑玻璃拟态 -->
      <div v-else v-scroll-reveal
        class="relative py-32 flex flex-col items-center justify-center rounded-[3rem] border border-white/5 bg-zinc-950/50 overflow-hidden shadow-2xl">
        <!-- 内部装饰光晕 -->
        <div
          class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 size-64 bg-blue-600/5 blur-[100px] pointer-events-none">
        </div>

        <div class="relative z-10 text-center">
          <div class="inline-flex p-6 rounded-full bg-white/5 border border-white/10 mb-8 text-zinc-600">
            <PackageOpen class="w-12 h-12" />
          </div>
          <h3 class="text-2xl font-bold text-white mb-2 tracking-tight">Stay Tuned</h3>
          <p class="text-zinc-500 max-w-[300px] mx-auto font-light leading-relaxed">
            New high-quality products are currently being added to the <span class="text-zinc-300 capitalize">{{
              categoryName }}</span> collection.
          </p>
          <div class="mt-10">
            <NuxtLink to="/products"
              class="text-[10px] font-bold uppercase tracking-[0.2em] text-blue-500 border-b border-blue-500/30 pb-1 hover:border-blue-500 transition-all">
              Back to all categories
            </NuxtLink>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup lang="ts">
import { PackageOpen } from 'lucide-vue-next'

const route = useRoute()
const categoryName = route.params.category as string

const { data: products } = await useAsyncData(`category-${categoryName}`, () => {
  // 使用新的 queryCollection 语法
  return queryCollection('products')
    .where('path', 'LIKE', `/products/${categoryName}/%`)
    .all()
})
// --- 动态 SEO 设置 ---
// 将 categoryName 格式化为标题，例如 "laser-therapy" -> "Laser Therapy"
const formattedCategory = computed(() =>
  categoryName.split('-').map(word => word.charAt(0).toUpperCase() + word.slice(1)).join(' ')
)

useSeoMeta({
  title: `${formattedCategory.value} | Product Collections`,
  description: `Explore our collection of ${formattedCategory.value}. High-quality products for your business needs.`,
  ogTitle: `${formattedCategory.value} | Product Collections`,
  ogDescription: `Explore our collection of ${formattedCategory.value}. High-quality products for your business needs.`
})
</script>

<style scoped>
/* 标题平衡 */
h1 {
  text-wrap: balance;
}

/* 自定义卡片容器阴影 */
.grid>div {
  transition: transform 0.3s ease;
}

/* 兼容低端浏览器的背景 */
.bg-zinc-950\/50 {
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
}
</style>