<template>
    <div class="relative min-h-screen bg-background flex flex-col overflow-hidden">

        <div class="w-full h-px bg-gradient-to-r from-transparent via-foreground/20 to-transparent"></div>

        <!-- 顶部文字区域：占据上半屏 -->
        <section  v-scroll-group class="relative z-10 flex-1 flex flex-col items-center justify-center px-4">
            <h1
           
                class="grow-text text-[56px] md:text-[120px] font-normal leading-[1.1] tracking-[-0.04em] text-transparent bg-clip-text">
                {{ siteConfig?.index?.hero?.welcomeText }}
            </h1>

            <p class="text-hero-sub text-center text-lg md:text-xl leading-relaxed max-w-md mt-6 opacity-80 font-light">
                {{ siteConfig?.index?.hero?.mainHeading }}
            </p>

            <div class="mt-10">
                <PremiumButton  variant="glass" size="lg" withArrow to="/products">
                 {{ siteConfig?.index?.hero?.ctaButton }}
                </PremiumButton>
            </div>
        </section>

        <!-- 底部视频区域：严格占据 50vh (屏幕高度的一半) -->
        <section class="relative w-full h-[50vh] min-h-[350px] overflow-hidden">
            <!-- 视频背景 -->
            <div class="absolute inset-0 z-0">
                <video ref="videoRef" autoplay muted playsinline
                    class="w-full h-full object-cover transition-opacity duration-500"
                    :style="{ opacity: videoOpacity }"
                    src="https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20260308_114720_3dabeb9e-2c39-4907-b747-bc3544e2d5b7.mp4"
                    @ended="handleVideoLoop"></video>
                <!-- 渐变遮罩：确保视频顶部与上方背景融合，底部保持深色 -->
                <div class="absolute inset-0 bg-gradient-to-b from-background via-transparent to-background"></div>
            </div>

            <!-- 内容层：h-full + justify-end 确保内容靠下 -->
            <div class="relative z-10 h-full flex flex-col justify-end pb-16 px-4">
                <!-- Logo Marquee -->
                <div v-scroll-reveal class="w-full mb-30 max-w-5xl mx-auto flex flex-col md:flex-row items-center gap-8 md:gap-12">
                    <div
                        class="text-foreground/50 text-sm whitespace-nowrap shrink-0 text-center md:text-left leading-relaxed font-light">
                        {{ siteConfig?.index?.hero?.logoMarqueeFirst }} <br class="hidden md:block" /> {{ siteConfig?.index?.hero?.logoMarqueeSecond }}
                    </div>

                    <div class="overflow-hidden flex-1 relative flex items-center">
                        <div class="flex animate-marquee gap-16 whitespace-nowrap">
                            <div v-for="i in 2" :key="i" class="flex gap-16 items-center">
                                <div v-for="brand in siteConfig?.index?.hero?.brands" :key="brand" class="flex items-center gap-3" >
                                    <div
                                        class="liquid-glass w-7 h-7 rounded-lg flex items-center justify-center font-bold text-xs">
                                        {{ brand[0] }}
                                    </div>
                                    <span class="text-base font-semibold text-foreground/90 tracking-tight">{{ brand }}</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>
<script setup lang="ts">
const videoRef = ref<HTMLVideoElement | null>(null)
const videoOpacity = ref(0)

// 从app.config.ts获取配置
const siteConfig = useAppConfig()
// 视频淡入淡出逻辑
const handleVideoLoop = () => {
    videoOpacity.value = 0
    setTimeout(() => {
        if (videoRef.value) {
            videoRef.value.currentTime = 0
            videoRef.value.play()
        }
    }, 100)
}

const updateFade = () => {
    if (!videoRef.value) return
    const v = videoRef.value
    const fadeTime = 0.5 // 秒

    // 淡入
    if (v.currentTime < fadeTime) {
        videoOpacity.value = v.currentTime / fadeTime
    }
    // 淡出
    else if (v.currentTime > v.duration - fadeTime) {
        videoOpacity.value = (v.duration - v.currentTime) / fadeTime
    }
    // 保持可见
    else {
        videoOpacity.value = 1
    }
    requestAnimationFrame(updateFade)
}

onMounted(() => {
    requestAnimationFrame(updateFade)
})
</script>

<style scoped>
.grow-text {
    background-image: linear-gradient(223deg, #E8E8E9 0%, #3A7BBF 104.15%);
    /* 确保字体在不同环境下显示一致 */
    font-family: 'General Sans', system-ui, sans-serif;
}

/* 隐藏滚动条 */
.animate-marquee {
    will-change: transform;
}
</style>