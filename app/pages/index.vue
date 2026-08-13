<template>
  <div>
    <HeroBlock />
     <CaseBlock />
     <BenefitsGridBlock />
    <FaqBlock />
    <CartBlock />
  </div>
</template>

<script setup>
const siteConfig = ref({

})
onMounted(async () => {
    try {
        const response = await fetch('/setting.json')
        if (response.ok) {
            const data = await response.json()
            siteConfig.value = { ...siteConfig.value, ...data }
        }
    } catch (e) {
        console.warn("Setting.json not found")
    }
})
// seo meta
useSeoMeta({
  title: 'Jinglan Sourcing',
  description: 'Jinglan Sourcing helps global buyers find reliable manufacturers in China, negotiate competitive prices, and manage sourcing from supplier selection to delivery.',
  ogTitle: 'Jinglan Sourcing',
  ogDescription: 'Jinglan Sourcing helps global buyers find reliable manufacturers in China, negotiate competitive prices, and manage sourcing from supplier selection to delivery.',
  ogImage: '/default-og.jpg',
  twitterCard: 'summary_large_image',
})
</script>

<style scoped>
/* styles */
</style>