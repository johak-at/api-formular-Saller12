<script setup>
import {ref} from "vue"
import NameInput from './NameInput.vue'
const userName = ref()
function setUserName(name)
{
    userName.value = name
}
</script>

<template>
    <NameInput @set-user-name="setUserName" v-if="!userName"></NameInput>
    <div v-else> 
        <h1>Hawidere {{userName}}</h1>
    </div>
</template>