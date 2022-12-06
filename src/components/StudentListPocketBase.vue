<script setup>
import { ref} from "vue";
import { onMounted } from 'vue';
import Pocketbase from 'pocketbase'

const pb = new Pocketbase('https://pb.seiwald.club')

const studentlist = ref([]);
onMounted(async () => {
    getStudents();
});
let schuelerVorname= ref("");
let schuelerNachname= ref("");
let schuelerwohnort= ref("");
let schuelergeburtstag= ref(null);
async function addStudent() {
    const newStudent = await pb.collection('4bhk').create({
            vorname: schuelerVorname.value,
            nachname: schuelerNachname.value,
            wohnort: schuelerwohnort.value,
            geburtstag: schuelergeburtstag.value,
    });
    getStudents();
}
async function getStudents(){
const data = await pb.collection('4bhk').getFullList();
studentlist.value = data;

}

async function deleteStudent(id){
    await pb.collection('4bhk').delete(id);
    getStudents();
}
</script>

<template>
    <div>     
        <h1>Our Students</h1>
        <ul>
        <li v-for="student in studentlist" :key="student.id">
            {{ student.vorname }}, {{ student.nachname }} wohnt in {{ student.wohnort }} und ist am {{ student.geburtstag.substring(0,10) }} geboren.
            <button @click="deleteStudent(student.id)">X</button>
        </li>
        </ul>
    </div>
    <form>
    <input type="text" v-model="schuelerVorname" placeholder="Vorname"/>
    <input type="text" v-model="schuelerNachname" placeholder="Nachname"/>
    <input type="text" v-model="schuelerwohnort" placeholder="Wohnort"/>
    <input type="date" v-model="schuelergeburtstag" placeholder="Geburtsdatum"/>
    <button @click.prevent="addStudent">Add Student</button>
</form>
</template>