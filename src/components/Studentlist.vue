<script setup>
import { ref} from "vue";
import { onMounted } from 'vue';
const baseUrl= "https://pb.seiwald.club/api/collections/4bhk/records"
const studentlist = ref([]);
onMounted(async () => {
    getStudents();
});
let schuelerVorname= ref("");
let schuelerNachname= ref("");
let schuelerwohnort= ref("");
let schuelergeburtstag= ref(null);
async function addStudent() {
    const newStudent = {
            vorname: schuelerVorname.value,
            nachname: schuelerNachname.value,
            wohnort: schuelerwohnort.value,
            geburtstag: schuelergeburtstag.value,
        }
    const response = await fetch(baseUrl, {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
        },
        body: JSON.stringify(newStudent)
    });
}
async function getStudents(){
    const response = await fetch(baseUrl)
const data = await response.json();
studentlist.value = data.items;

}

async function deleteStudent(id){
    await fetch(`${baseUrl}/${id}`, {
        method: "DELETE",
    });
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