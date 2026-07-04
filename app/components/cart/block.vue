<template>
  <section class="py-12 px-6 bg-black">
    <div v-scroll-reveal="{ delay: 0.2,y: 80}" class="container mx-auto max-w-[860px]">
      <!-- 主容器：带有深色渐变和边线 -->
      <div
        class="relative overflow-hidden rounded-[2.5rem] border border-white/10 bg-zinc-950 px-8 py-10 md:py-20 text-center">

        <!-- 1. 背景光晕装饰：将原图紫色换成您的蓝色 -->
        <!-- 左上角光晕 -->
        <div class="absolute -top-24 -left-24 size-96 bg-blue-600/10 blur-[100px] pointer-events-none"></div>
        <!-- 右下角光晕 -->
        <div class="absolute -bottom-24 -right-24 size-96 bg-blue-500/10 blur-[100px] pointer-events-none"></div>

        <div class="relative z-10 flex flex-col items-center">
          <!-- 2. 主标题：高度还原字间距和行高 -->
          <h2
            class="text-[28px] md:text-[40px] font-light leading-[1.1] tracking-[-0.04em] mb-6 mx-auto">
           {{ siteConfig?.index?.cart?.mainHeading }}
          </h2>

          <!-- 3. 副标题 -->
          <p class="mt-8 text-zinc-400 text-lg font-extralight opacity-90  mx-auto">
            {{ siteConfig?.index?.cart?.subtitle }}
          </p>

          <!-- 4. 核心 CTA 按钮：改为您的蓝色方案 -->
          <div class="mt-12">
            <PremiumButton @click="onInquiry" variant="glass" size="lg" withArrow class="w-full">
              Inquiry Now
            </PremiumButton>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
const siteConfig = useAppConfig()
const { openInquiry } = useInquiry()
// 从app.config.ts获取配置


// 打开咨询弹窗
const onInquiry = () => {
    // 默认就是 true，也可以显式写出 
    //showLabel: false 表示不显示咨询弹窗的标题
    // 第一个参数配置如咨询的商品名称
    openInquiry("", { showLabel: false })
}
</script>

<style scoped>
/* 确保标题在任何宽度下都有良好的平衡感 */
h2 {
  text-wrap: balance;
}

/* 模拟图片中那种深邃且带有微弱磨砂感的背景 */
.bg-zinc-950 {
  background-image: radial-gradient(circle at center,
      rgba(255, 255, 255, 0.02) 0%,
      transparent 100%);
}
</style>