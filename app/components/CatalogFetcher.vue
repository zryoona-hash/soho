<template>
  <div class="w-full max-w-sm">
    <h4 v-if="siteConfig?.footer?.emailmessage" class="text-white text-xs font-light uppercase tracking-[0.2em] mb-6 inline-block pb-1">
      {{ siteConfig?.footer?.emailmessage }}
    </h4>

    <!-- 修正 1: 增加 form 标签 -->
    <form @submit.prevent="onSubmit" class="space-y-2">
      <div class="relative flex items-center p-1 bg-white/5 border border-white/10 rounded-2xl focus-within:border-blue-500/50 transition-all duration-300"
           :class="{ 'border-red-500/50': errors.email }">
        
        <input 
          v-model="email" 
          v-bind="emailAttrs" 
          type="email" 
          placeholder="name@email.com" 
          class="flex-1 bg-transparent border-none text-white text-sm px-4 py-2.5 outline-none placeholder:text-zinc-700"
        />
        
        <button 
          type="submit" 
          :disabled="isSubmitting" 
          class="group flex items-center gap-2 bg-blue-600 hover:bg-blue-500 disabled:bg-zinc-700 text-white px-6 py-2.5 rounded-full font-bold text-xs uppercase tracking-wider transition-all active:scale-95 shadow-lg shadow-blue-600/20"
        >
          <template v-if="isSubmitting">
            <Loader2 class="w-4 h-4 animate-spin" />
            <span>Sending...</span>
          </template>
          <template v-else>
            <span>Send</span>
            <ArrowRight class="size-4 transition-transform group-hover:translate-x-1" />
          </template>
        </button>
      </div>

      <!-- 修正 2: 增加错误信息显示 -->
      <p v-if="errors.email" class="text-red-400 text-xs mt-1 ml-4 italic">
        {{ errors.email }}
      </p>
    </form>
  </div>
</template>

<script lang='ts' setup>
import { useForm } from 'vee-validate';
import { Loader2, ArrowRight } from 'lucide-vue-next';
import { categorySchema } from '@/utils/categotyValidation';

// 假设这些是从全局注入或 Props 传入
const config = useRuntimeConfig();
const toast = useToast();
const siteConfig = useAppConfig(); // 确保 siteConfig 已定义

const accessKey = config.public.web3FormsKey;

// 1. 表单初始化
const { defineField, handleSubmit, errors, isSubmitting, resetForm } = useForm({
    validationSchema: categorySchema,
    initialValues: { 
        email: '', 
        subject: '来自网站的请求' // 更有意义的默认主题
    }
});

const [email, emailAttrs] = defineField('email');

// 2. 修正后的提交逻辑
const onSubmit = handleSubmit(async (values) => {
    if (!accessKey) {
        toast.error('Configuration error: Missing Access Key');
        return;
    }

    try {
        const response = await fetch("https://api.web3forms.com/submit", {
            method: "POST",
            headers: { 
                "Content-Type": "application/json", 
                "Accept": "application/json" 
            },
            body: JSON.stringify({
                ...values,
                access_key: accessKey,
                from_name: "Website Catalog Fetcher",
                subject: `New Catalog Request from ${values.email}`
            })
        });

        const data = await response.json();
        
        if (data.success) {
            toast.success('Success! The catalog link has been sent to your email.');
            resetForm(); // 提交成功后重置表单
        } else {
            throw new Error(data.message || 'Submission failed');
        }
    } catch (error: any) {
        toast.error(error.message || 'Failed to send request');
    }
});
</script>