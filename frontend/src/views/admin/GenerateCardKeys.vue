<template>
    <div>
        <n-card>
            <n-form @submit.prevent="generateKeys">
                <n-form-item label="生成卡密数量">
                    <n-input v-model="count" type="number" />
                </n-form-item>
                <n-button type="primary" html-type="submit" @click="generateKeys">生成</n-button>
            </n-form>
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

const count = ref(1);
const keys = ref([]);
const message = useMessage();

const generateKeys = async () => {
    console.log('Response:', 1);
    try {
        console.log('Sending request to generate card keys...');
        const res = await api.fetch('/admin/generate_card_keys', {
            method: 'POST',
            body: JSON.stringify({ count: count.value })
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
