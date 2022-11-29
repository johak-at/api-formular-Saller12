<script setup>
import { ref} from "vue";
import { onMounted } from 'vue';
const studentlist = ref(null);
onMounted(async () => {
const response = await fetch("https://pb.seiwald.club/api/collections/4bhk/records")
const data = await response.json();
});
</script>

<template>
    <div>
        <h1>Our Students</h1>
        <ul>
        <li v-for="student in students" :key="student.id">
            {{ student.name }}
        </li>
        </ul>
    </div>
</template>