<template>
    <div>
        <n-card>
            <n-form-item label="生成卡密数量">
                <n-input v-model="count" />
            </n-form-item>
            <n-button type="primary" @click="generateKeys">生成</n-button>
        </n-card>
        <n-card v-if="keys.length">
            <n-list>
                <n-list-item v-for="key in keys" :key="key">{{ key }}</n-list-item>
            </n-list>
        </n-card>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { useMessage } from 'naive-ui';
import { api } from '../../api';

const count = ref(Number(1));
const keys = ref([]);
const message = useMessage();

const generateKeys = async () => {
    console.log('Current count:', count.value);
    try {
        const currentCount = count.value; // 获取当前值
        console.log('Sending request with count:', currentCount);
        const res = await api.fetch('/admin/generate_card_keys', {
            method: 'POST',
            body: JSON.stringify({ count: 10 }),
            headers: {
                'Content-Type': 'application/json'
            }
        });
        console.log('Response:', res);
        keys.value = res.keys;
        message.success('卡密生成成功');
    } catch (error) {
        console.error('Error:', error);
        message.error(error.message || '生成卡密失败');
    }
};
</script>
