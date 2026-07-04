<template>
    <Teleport to="body">
        <!-- 弹窗遮罩层 -->
        <div v-if="isOpen" class="fixed inset-0 z-[150] flex items-center justify-center p-4">
            <!-- 更加深邃的背景遮罩 -->
            <div class="absolute inset-0 bg-black/80 backdrop-blur-md" @click="handleClose"></div>

            <!-- 弹窗主体：暗黑玻璃拟态 UI -->
            <div
                class="relative liquid-glass bg-zinc-950  shadow-[0_0_100px_-20px_rgba(0,0,0,0.8)] border border-white/10 rounded-[2.5rem]  w-full max-w-lg p-10 overflow-hidden animate-in fade-in zoom-in duration-300">

                <!-- 内部光晕装饰 -->
                <div class="absolute -top-24 -left-24 size-48 bg-primary/10 blur-[80px] pointer-events-none"></div>

                <div class="relative z-10">
                    <h2 class="text-3xl font-bold text-white mb-2 tracking-tight">Send Inquiry</h2>

                    <!-- Regarding 逻辑保持 -->
                    <p v-if="showRegarding && productName" class="text-zinc-400 mb-8 text-sm flex items-center gap-2">
                        Regarding: <span
                            class="px-2 py-0.5 rounded-lg bg-white/5 text-zinc-200 border border-white/5 font-medium">{{
                            productName }}</span>
                    </p>

                    <form @submit="onSubmit" class="space-y-5" novalidate>
                        <!-- Name -->
                        <div class="space-y-1.5">
                            <label
                                class="block text-xs font-bold text-zinc-500 uppercase tracking-widest ml-1">Name</label>
                            <input v-model="name" v-bind="nameAttrs" type="text"
                                :class="errors.name ? 'border-red-500/50 bg-red-500/5' : 'border-white/10 bg-white/5 focus:border-primary/50'"
                                class="w-full px-4 py-3.5 rounded-2xl border text-white placeholder:text-zinc-600 outline-none transition-all duration-300"
                                placeholder="John Doe">
                            <p v-if="errors.name"
                                class="text-red-500 text-[10px] mt-1 ml-1 uppercase font-bold tracking-wider">{{
                                errors.name }}</p>
                        </div>

                        <!-- Email -->
                        <div class="space-y-1.5">
                            <label
                                class="block text-xs font-bold text-zinc-500 uppercase tracking-widest ml-1">Email</label>
                            <input v-model="email" v-bind="emailAttrs" type="email"
                                :class="errors.email ? 'border-red-500/50 bg-red-500/5' : 'border-white/10 bg-white/5 focus:border-primary/50'"
                                class="w-full px-4 py-3.5 rounded-2xl border text-white placeholder:text-zinc-600 outline-none transition-all duration-300"
                                placeholder="john@example.com">
                            <p v-if="errors.email"
                                class="text-red-500 text-[10px] mt-1 ml-1 uppercase font-bold tracking-wider">{{
                                errors.email }}</p>
                        </div>

                        <!-- Message -->
                        <div class="space-y-1.5">
                            <label
                                class="block text-xs font-bold text-zinc-500 uppercase tracking-widest ml-1">Message</label>
                            <textarea v-model="message" v-bind="messageAttrs" rows="4"
                                :class="errors.message ? 'border-red-500/50 bg-red-500/5' : 'border-white/10 bg-white/5 focus:border-primary/50'"
                                class="w-full px-4 py-3.5 rounded-2xl border text-white placeholder:text-zinc-600 outline-none transition-all duration-300 resize-none"
                                placeholder="How can we help you?"></textarea>
                            <p v-if="errors.message"
                                class="text-red-500 text-[10px] mt-1 ml-1 uppercase font-bold tracking-wider">{{
                                errors.message }}</p>
                        </div>

                        <!-- 提交按钮：采用 Skal 风格的高对比按钮 -->
                        <button type="submit" :disabled="isSubmitting"
                            class="w-full cursor-pointer mt-4 group relative overflow-hidden bg-white text-black hover:bg-zinc-200 disabled:opacity-50 disabled:cursor-not-allowed py-4 rounded-full font-bold text-base transition-all duration-300 active:scale-[0.98] flex items-center justify-center gap-3">
                            <template v-if="isSubmitting">
                                <Loader2 class="w-5 h-5 animate-spin" />
                                <span>Sending...</span>
                            </template>
                            <template v-else>
                                <span>Send Inquiry Now</span>
                                <ArrowRight class="size-5 transition-transform group-hover:translate-x-1" />
                            </template>
                        </button>
                    </form>
                </div>

                <!-- 关闭按钮：精致的悬浮效果 -->
                <button @click="handleClose" type="button"
                    class="absolute border cursor-pointer top-6 right-6 size-10 flex items-center justify-center rounded-full border-white/20 text-zinc-500 hover:text-white bg-white/10 hover:border-white/40 transition-all duration-300 z-50">
                    <!-- 2. 给图标加上 pointer-events-none，确保它不干扰鼠标事件 -->
                    <X class="size-5 pointer-events-none" />
                </button>
            </div>
        </div>
    </Teleport>
</template>

<script setup lang="ts">
import { useForm } from 'vee-validate';
import { contactSchema } from '~/utils/validation';
import { Loader2, X, ArrowRight } from 'lucide-vue-next';
const config = useRuntimeConfig()
const { isOpen, productName, showRegarding, closeInquiry, openInquiry } = useInquiry()
// 获取这个 key
const accessKey = config.public.web3FormsKey

const toast = useToast()
const siteConfig = ref<any>({ contact: {} })
const { defineField, handleSubmit, errors, isSubmitting, resetForm } = useForm({
    validationSchema: contactSchema,
    initialValues: { name: '', email: '', message: '', subject: 'Pending...' }
});

const [name, nameAttrs] = defineField('name');
const [email, emailAttrs] = defineField('email');
const [message, messageAttrs] = defineField('message');


const handleClose = () => {
    resetForm();
    closeInquiry();
};
const onSubmit = handleSubmit(
    // 1. 成功回调：校验通过执行此段
    async (values) => {
        // console.log('提交触发，配置项:', siteConfig.value);

        if (!accessKey) {
            toast.error('Configuration not loaded yet, please try again.');
            return;
        }
        const payload = {
            ...values, // 这里包含了原来的 { name, email, message, subject: 'Pending...' }
            access_key: accessKey,
            subject: `Inquiry for ${productName.value}` // 这样它会覆盖掉 values 里的 'Pending...'
        };

        try {
            const response = await fetch("https://api.web3forms.com/submit", {
                method: "POST",
                headers: { "Content-Type": "application/json", "Accept": "application/json" },
                body: JSON.stringify(payload)
            });

            const data = await response.json();
            if (data.success) {
                toast.success('Inquiry sent successfully!');
                handleClose();
            } else {
                throw new Error(data.message || 'Submission failed');
            }
        } catch (error: any) {

            toast.error(error.message || 'Failed to send inquiry');
        }
    },
    // 2. 失败回调：校验未通过执行此段
    (errors) => {
        console.log('校验失败:', errors);
        toast.error('Please check your input fields.');
    }
);
onMounted(async () => {
    try {
        const response = await fetch('/setting.json')
        if (response.ok) siteConfig.value = await response.json()
    } catch (e) { console.warn("Setting.json not found") }
})
</script>