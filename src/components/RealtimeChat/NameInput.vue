<script setup>
import {ref} from 'vue';
const emits = defineEmits(['setUserName']);
const userName = ref('');
</script>

<template>
    <h2>Servas Hawara! Wos is dei nom?</h2>
    <input type="text" v-model="userName"  @keyup.enter="emits('setUserName', userName)"/>
    <button @click="emits('setUserName', userName)">Sendn</button>
</template>