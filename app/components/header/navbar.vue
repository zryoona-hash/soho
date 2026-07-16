<template>
    <nav :class="[
        'fixed top-0 left-0 right-0 z-[100] transition-all duration-700 ease-[cubic-bezier(0.25,1,0.5,1)]',
        isScrolled
            ? 'bg-background/80 backdrop-blur-2xl border-b border-white/10 shadow-2xl py-0'
            : 'bg-transparent py-2'
    ]">
        <div class="container  px-6 max-w-[1200px] mx-auto">
            <div class="flex items-center justify-between h-20">

                <!-- 1. Logo Area -->
                <div class="transition-all duration-500" :class="isScrolled ? 'scale-95' : 'scale-100'">
                    <NuxtLink to="/" class="flex items-center gap-3 group">
                        <div class="flex w-10 h-10 items-center justify-center">
                           <img v-if="siteConfig.logo?.logoUrl" :src="siteConfig.logo?.logoUrl" alt="Logo" class="w-full h-full object-contain">
                              
                        </div>
                        <span class="text-xl font-bold text-white tracking-tighter  whitespace-nowrap">
                            {{ siteConfig?.logo?.logoName }}
                        </span>
                    </NuxtLink>
                </div>

                <!-- 2. Desktop Navigation -->
                <div class=" hidden md:flex items-center h-full gap-2">

                    <div v-for="(item, index) in navItems" :key="index"
                        class="relative group h-full flex  items-center">

                        <!-- 导航项按钮 -->
                        <NuxtLink :to="item.href"
                            class="text-foreground/70 hover:text-white transition-all px-4 py-2 rounded-full hover:bg-white/5 font-medium text-md flex items-center gap-1.5">
                            {{ item.label }}
                            <ChevronDown v-if="item.hasDropdown"
                                class="w-3.5 h-3.5 opacity-40 group-hover:rotate-180 group-hover:opacity-100 transition-all duration-500" />
                        </NuxtLink>

                        <!-- 下拉菜单：核心修正位置 -->
                        <div v-if="item.hasDropdown" class="absolute top-full left-1/2 -translate-x-1/2 w-52 pt-4
    opacity-0 invisible group-hover:opacity-100 group-hover:visible 
    transition-all duration-300 transform translate-y-2 group-hover:translate-y-0 z-[200]">
                            <!-- 1. 隐形连接层（桥梁）：防止鼠标移向菜单时消失 -->
                            <div class="absolute top-0 left-0 w-full h-4"></div>

                            <!-- 2. 实际内容卡片 -->
                            <div
                                class="liquid-glass bg-background/95 border border-white/10 rounded-2xl shadow-2xl overflow-hidden">
                                <NuxtLink v-for="(subItem, subIndex) in item.subItems" :key="subIndex"
                                    :to="subItem.href"
                                    class="block px-5 py-2.5 text-sm bg-background/80  text-foreground/70 hover:text-white hover:background transition-colors font-normal">
                                    {{ subItem.label }}
                                </NuxtLink>
                            </div>
                        </div>
                    </div>

                </div>

                <!-- 3. Desktop Action Button -->
                <div class="hidden md:flex items-center">
                    <PremiumButton @click="onInquiry" variant="primary" size="sm" withArrow>
                        Inquiry Now
                    </PremiumButton>
                </div>

                <!-- 4. Mobile Menu Toggle -->
                <div class="md:hidden flex items-center">
                    <button @click="mobileMenuOpen = !mobileMenuOpen"
                        class="text-white p-2.5 rounded-full bg-white/5 hover:bg-white/10 transition-colors focus:outline-none"
                        aria-label="Toggle Menu">
                        <X v-if="mobileMenuOpen" class="w-6 h-6" />
                        <svg v-else xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none" viewBox="0 0 24 24"
                            stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                                d="M4 8h16M8 16h12" />
                        </svg>
                    </button>
                </div>

                <!-- 5. Mobile Menu Overlay -->
                <Transition enter-active-class="transition-all duration-500 ease-out"
                    enter-from-class="opacity-0 translate-y-4" enter-to-class="opacity-100 translate-y-0"
                    leave-active-class="transition-all duration-300 ease-in"
                    leave-from-class="opacity-100 translate-y-0" leave-to-class="opacity-0 translate-y-4">
                    <div v-if="mobileMenuOpen"
                        class="md:hidden fixed inset-0 top-[70px] z-[99] bg-background/95 backdrop-blur-3xl overflow-y-auto px-6 py-10">
                        <div class="flex flex-col gap-2">
                            <div v-for="(item, index) in navItems" :key="index" class="w-full">
                                <div class="flex items-center justify-between py-4">
                                    <NuxtLink :to="item.href" @click="mobileMenuOpen = false"
                                        class="text-2xl font-bold text-white tracking-tight">
                                        {{ item.label }}
                                    </NuxtLink>

                                    <button v-if="item.hasDropdown" @click="toggleMobileDropdown(index)"
                                        class="p-2 bg-white/5 rounded-full text-white/50">
                                        <ChevronDown :class="{ 'rotate-180 text-primary': mobileDropdowns[index] }"
                                            class="w-5 h-5 transition-transform duration-500" />
                                    </button>
                                </div>

                                <!-- 移动端子菜单 -->
                                <div v-if="item.hasDropdown && mobileDropdowns[index]"
                                    class="mb-4 ml-4 flex flex-col gap-4 border-l border-white/10 pl-6 py-2">
                                    <NuxtLink v-for="(subItem, subIndex) in item.subItems" :key="subIndex"
                                        :to="subItem.href" @click="mobileMenuOpen = false"
                                        class="text-lg text-foreground/60 hover:text-white transition-colors">
                                        {{ subItem.label }}
                                    </NuxtLink>
                                </div>
                            </div>

                            <div class="mt-12">
                                <PremiumButton @click="onInquiry" variant="primary" size="lg" withArrow class="w-full">
                                    Inquiry Now
                                </PremiumButton>
                            </div>
                        </div>
                    </div>
                </Transition>
            </div>
        </div>
    </nav>
</template>

<script setup lang="ts">

import { ref, onMounted, reactive, computed, watch, onUnmounted } from 'vue'
import {
    X, ChevronDown, ArrowRight,
} from 'lucide-vue-next'
const isScrolled = ref(false)
const { openInquiry } = useInquiry()
// 从app.config.ts获取配置
const siteConfig = useAppConfig()

// 打开咨询弹窗
const onInquiry = () => {
    // 默认就是 true，也可以显式写出 
    //showLabel: false 表示不显示咨询弹窗的标题
    // 第一个参数配置如咨询的商品名称
    openInquiry("", { showLabel: false })
}
let ticking = false;
const handleScroll = () => {
    if (!ticking) {
        window.requestAnimationFrame(() => {
            isScrolled.value = window.scrollY > 20
            ticking = false
        })
        ticking = true
    }
}

// --- 1. 基础状态定义 ---
// const isModalOpen = ref(false)

const mobileMenuOpen = ref(false)
const mobileDropdowns = ref<Record<number, boolean>>({})


const { data: navData } = await useProductsNav()
// 2. 确保计算属性安全读取
const navItems = computed(() => {
    // 1. 安全获取数据
    const rootNode = navData.value?.[0]
    if (!rootNode) {
        // 如果数据还没回来，返回基础菜单，防止页面空白
        return [
            { label: 'Home', href: '/' },
            { label: 'About', href: '/about' },
            { label: 'Contact', href: '/contact' }
        ]
    }

    // 2. 将 Content 树形结构映射为你的导航格式
    // 你的数据结构是: root(Products) -> children(Industrial, Electronics)
    const productMenu = {
        label: rootNode.title, // "Products"
        href: rootNode.path,   // "/products"
        hasDropdown: true,
        subItems: rootNode.children?.map(child => ({
            label: child.title, // "Industrial"
            href: child.path    // "/products/industrial"
        })) || []
    }

    // 3. 组合菜单
    return [
        { label: 'Home', href: '/' },
        productMenu,
        { label: 'About', href: '/about' },
        { label: 'Contact', href: '/contact' }
    ]
})


// --- 3. 其他功能函数 ---
const toggleMobileDropdown = (index: number) => {
    mobileDropdowns.value[index] = !mobileDropdowns.value[index]
}


watch(mobileMenuOpen, (isOpen) => {
    if (typeof window !== 'undefined') {
        if (isOpen) {
            // 禁止 body 滚动
            document.body.style.overflow = 'hidden'
            document.body.style.position = 'fixed'
            document.body.style.width = '100%'
        } else {
            // 恢复 body 滚动
            document.body.style.overflow = ''
            document.body.style.position = ''
            document.body.style.width = ''
        }
    }
})


onMounted(async () => {
    window.addEventListener('scroll', handleScroll)
})
onUnmounted(() => {
    // 记得销毁监听，防止内存泄漏
    window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
@keyframes modalIn {
    from {
        opacity: 0;
        transform: scale(0.9) translateY(20px);
    }

    to {
        opacity: 1;
        transform: scale(1) translateY(0);
    }
}

.animate-modal-in {
    animation: modalIn 0.4s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

.animate-slide-in {
    animation: slideIn 0.3s ease-out forwards;
}

@keyframes slideIn {
    from {
        opacity: 0;
        transform: translateX(-15px);
    }

    to {
        opacity: 1;
        transform: translateX(0);
    }
}
</style>