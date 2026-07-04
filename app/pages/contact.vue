<template>
  <section class="bg-black text-white py-24 px-6 font-sans overflow-hidden">
    <div class="container mx-auto max-w-[960px]">
      
      <!-- 1. 头部区域 (延用你的 Benefits 风格) -->
      <div class="flex flex-col items-center text-center mb-16">
        <div v-scroll-reveal class="px-4 py-1.5 rounded-lg border border-zinc-800 bg-zinc-900/50 text-[12px] font-medium text-zinc-400 mb-8 uppercase tracking-widest">
         {{ siteConfig?.contact?.tag || 'Contact Us' }}
        </div>
        <h1 v-scroll-reveal="{ delay: 0.1 }" class="text-[42px] md:text-[64px] font-light leading-[1.1] tracking-[-0.03em] mb-6">
         {{ siteConfig?.contact?.title || 'Start your next' }} <span class="text-blue-500">{{ siteConfig?.contact?.sptext || 'project' }}</span>
        </h1>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-12 gap-12 items-start">
        
        <!-- 2. 左侧：联系信息 -->
        <div class="lg:col-span-5 space-y-8">
          <div v-scroll-reveal="{ delay: 0.2 }" class="relative p-8 rounded-2xl border border-white/10 bg-[#050505] overflow-hidden group">
            <div class="absolute -bottom-1 left-0 right-0 h-24 bg-gradient-to-t from-blue-600/10 to-transparent opacity-50"></div>
            
            <h3 class="text-2xl font-bold mb-6 tracking-tight">Contact Information</h3>
            
            <div class="space-y-6">
              <div v-for="(info, index) in siteConfig?.contact?.contactInfo || []" :key="index" class="flex items-start gap-4">
                <div class="p-3 rounded-xl bg-white/5 border border-white/10 text-blue-400">
                 <component :is="iconMap[info.icon]" v-if="iconMap[info.icon]" class="size-5" />
                </div>
                <div>
                  <p class="text-zinc-500 text-xs uppercase tracking-widest mb-1">{{ info.label }}</p>
                  <p class="text-white font-medium">{{ info.value }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- 3. 右侧：改造后的表单 -->
        <div v-scroll-reveal="{ delay: 0.3 }" class="lg:col-span-7">
          <div class="relative bg-[#050505] p-8 md:p-12 rounded-2xl border border-white/10 shadow-2xl overflow-hidden">
            <!-- 背景光晕装饰 -->
            <div class="absolute -top-24 -right-24 size-48 bg-blue-600/10 blur-[80px] pointer-events-none"></div>

            <form @submit.prevent="onSubmit" class="relative z-10 space-y-6" novalidate>
              <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Name -->
                <div class="space-y-2">
                  <label class="block text-xs font-bold text-zinc-500 uppercase tracking-widest ml-1">Name</label>
                  <input v-model="name" v-bind="nameAttrs" type="text"
                    :class="errors.name ? 'border-red-500/50 bg-red-500/5' : 'border-white/10 bg-white/5 focus:border-blue-500/50'"
                    class="w-full px-5 py-4 rounded-xl border text-white placeholder:text-zinc-700 outline-none transition-all duration-300"
                    placeholder="John Doe">
                  <p v-if="errors.name" class="text-red-500 text-[10px] mt-1 ml-1 font-bold uppercase tracking-wider">{{ errors.name }}</p>
                </div>
                <!-- Email -->
                <div class="space-y-2">
                  <label class="block text-xs font-bold text-zinc-500 uppercase tracking-widest ml-1">Email</label>
                  <input v-model="email" v-bind="emailAttrs" type="email"
                    :class="errors.email ? 'border-red-500/50 bg-red-500/5' : 'border-white/10 bg-white/5 focus:border-blue-500/50'"
                    class="w-full px-5 py-4 rounded-xl border text-white placeholder:text-zinc-700 outline-none transition-all duration-300"
                    placeholder="john@example.com">
                  <p v-if="errors.email" class="text-red-500 text-[10px] mt-1 ml-1 font-bold uppercase tracking-wider">{{ errors.email }}</p>
                </div>
              </div>

              <!-- Subject -->
              <div class="space-y-2">
                <label class="block text-xs font-bold text-zinc-500 uppercase tracking-widest ml-1">Subject</label>
                <input v-model="subject" v-bind="subjectAttrs" type="text"
                  :class="errors.subject ? 'border-red-500/50 bg-red-500/5' : 'border-white/10 bg-white/5 focus:border-blue-500/50'"
                  class="w-full px-5 py-4 rounded-xl border text-white placeholder:text-zinc-700 outline-none transition-all duration-300"
                  placeholder="Regarding product inquiry...">
                <p v-if="errors.subject" class="text-red-500 text-[10px] mt-1 ml-1 font-bold uppercase tracking-wider">{{ errors.subject }}</p>
              </div>

              <!-- Message -->
              <div class="space-y-2">
                <label class="block text-xs font-bold text-zinc-500 uppercase tracking-widest ml-1">Message</label>
                <textarea v-model="message" v-bind="messageAttrs" rows="5"
                  :class="errors.message ? 'border-red-500/50 bg-red-500/5' : 'border-white/10 bg-white/5 focus:border-blue-500/50'"
                  class="w-full px-5 py-4 rounded-xl border text-white placeholder:text-zinc-700 outline-none transition-all duration-300 resize-none"
                  placeholder="Tell us about your project..."></textarea>
                <p v-if="errors.message" class="text-red-500 text-[10px] mt-1 ml-1 font-bold uppercase tracking-wider">{{ errors.message }}</p>
              </div>

              <!-- 提交按钮：黑白高对比度风格 -->
              <button type="submit" :disabled="isSubmitting"
                class="w-full cursor-pointer mt-4 group relative overflow-hidden bg-white text-black hover:bg-zinc-200 disabled:opacity-50 disabled:cursor-not-allowed py-5 rounded-full font-bold text-sm uppercase tracking-widest transition-all duration-300 active:scale-[0.98] flex items-center justify-center gap-3">
                <template v-if="isSubmitting">
                  <Loader2 class="w-5 h-5 animate-spin" />
                  <span>Sending...</span>
                </template>
                <template v-else>
                  <span>Send Inquiry Now</span>
                  <Send class="w-4 h-4 transition-transform group-hover:translate-x-1 group-hover:-translate-y-1" />
                </template>
              </button>
            </form>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>



<script setup lang="ts">

import { ref, onMounted } from 'vue'
import { Mail, Phone, MapPin, Send, Loader2 } from 'lucide-vue-next'
import { useForm } from 'vee-validate';
import { contactSchema } from '~/utils/validation';
const config = useRuntimeConfig()

// 获取这个 key
const accessKey = config.public.web3FormsKey
const toast = useToast();
const siteConfig = useAppConfig()
const iconMap = {
  Mail,
  Phone,
  MapPin
}
// 1. 使用 vee-validate 管理状态
const { defineField, handleSubmit, errors, isSubmitting, resetForm } = useForm({
    validationSchema: contactSchema,
     initialValues: {
        name: '',
        email: '',
        subject: '',
        message: '',
    },
});

const [name, nameAttrs] = defineField('name',{validateOnModelUpdate: false});
const [email, emailAttrs] = defineField('email',{validateOnModelUpdate: false});
const [subject, subjectAttrs] = defineField('subject',{validateOnModelUpdate: false});
const [message, messageAttrs] = defineField('message',{validateOnModelUpdate: false});

// 2. 封装发送逻辑
const onSubmit = handleSubmit(async (values) => {
    

    try {
        const response = await fetch("https://api.web3forms.com/submit", {
            method: "POST",
            headers: { "Content-Type": "application/json", "Accept": "application/json" },
            body: JSON.stringify({ access_key: accessKey, ...values })
        });

        const data = await response.json();
        if (data.success) {
            toast.success('Inquiry sent successfully!');
            resetForm();
        } else {
            throw new Error(data.message);
        }
    } catch (error: any) {
        toast.error(error.message || 'Failed to send');
    }
});





// seo meta
useSeoMeta({
  title: () => siteConfig.value?.seo?.contact.title || 'Default Title',
  description: () => siteConfig.value?.seo?.contact.description || 'Default description',
  ogTitle: () => siteConfig.value?.seo?.contact.title || 'Default Title',
  ogDescription: () => siteConfig.value?.seo?.contact.description || 'Default description',
  ogImage: () => siteConfig.value?.seo?.contact.ogImage || '/default-og.jpg',
  twitterCard: () => siteConfig.value?.seo?.contact.twitterCard || 'summary_large_image',
})
</script>