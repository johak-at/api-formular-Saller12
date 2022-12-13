<script setup>
import {onMounted, ref} from "vue"
import NameInput from './NameInput.vue'
import pb from '@/database/pb.js'
const userName = ref()
const newMessage = ref("");
function setUserName(name)
{
    userName.value = name
}

const chatMessages = ref([])
onMounted( ()=>{
    loadChatMessages();
    pb.collection('rtc_4bhk').subscribe("*", function(e){
        loadChatMessages();
    });
});

async function loadChatMessages()
{
    const data = await pb.collection('rtc_4bhk').getFullList();
    chatMessages.value = data;
}

async function postMessage()
{
    console.log("message posted")
    const record = await pb.collection('rtc_4bhk').create({
        sender: userName.value,
        message: newMessage.value
    });
    newMessage.value = "";
}

</script>

<template>
    <NameInput @set-user-name="setUserName" v-if="!userName"></NameInput>
    <div v-else> 
        <h1>Hawidere {{userName}}</h1>
        <input type="text" placeholder="Type Message here" style="width:100%" v-model="newMessage" @keyup.enter="postMessage"/> 
        <ul>
            <li class="msg" :class="{self: message.sender === userName}" v-for="message in chatMessages" :key="message.id">
                <div style="fontSize: 0.7rem"> {{message.sender}}:</div>  <div>{{ message.message }}</div>
            </li>
        </ul>
    </div>
</template>

<style scoped>
    .msg{
        width: 200px;
        display: flex;
        flex-direction: column;
        color: red;
        text-align: left;
        list-style: none;
        line-height: 1.5;
        margin-bottom: 0.6rem
    }

    .self{
        text-align: right;
    }
</style>