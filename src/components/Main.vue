<template>
    <h1>Learning Schedule</h1>
    <div class="header">
        <div class="">
            <span>Subject</span>
            <input type="text" v-model="forum_data.subject" placeholder="Enter subject here">
        </div>
        <div class="">
            <span>Content</span>
            <input type="text" v-model="forum_data.content" placeholder="Enter content here">
        </div>
        <div class="">
            <span>Location</span>
            <input type="text" v-model="forum_data.location" placeholder="Select location">
        </div>
        <button @click="add">Add</button>
    </div>
    <div class="table">
        <table id="schedules">
            <thead>
                <tr>
                    <th>Subject</th>
                    <th>Content</th>
                    <th>Location</th>
                    <th>Completed</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="i, index in schedules" :key="index">
                    <td>{{ i.subject }}</td>
                    <td>{{ i.content }}</td>
                    <td>{{ i.location }}</td>
                    <td><input type="checkbox" id="checkbox" v-model="i.is_completed">{{ i.is_completed }}</td>
                    <td><a href="#" @click="remove_sch(index)">Delete</a></td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import NotifyX from 'notifyx'
import 'notifyx/dist/notifyx.min.css'

const forum_data = reactive({
    subject: '',
    content: '',
    location: ''
})

const schedules = ref([])

const remove_sch = (index) => {
    NotifyX.info('Removing schedule: ' + schedules.value[index].subject  + '...')
    schedules.value.splice(index, 1)
    NotifyX.success('Schedule removed')
}

const add = () => {
    if (forum_data.subject == '' || forum_data.content == '' || forum_data.location == '') {
        NotifyX.error('Please fill all the fields', {
            duration: 5000
        })
        return
    }

    schedules.value.push({
        subject: forum_data.subject,
        content: forum_data.content,
        location: forum_data.location,
        is_completed: false
    })
    NotifyX.success('Successfully added a schedule: ' + forum_data.subject)
    forum_data.subject = ''
    forum_data.content = ''
    forum_data.location = ''
}
</script>

<style scoped>
/* #region AI GENERATED STYLES */
.header {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    margin-bottom: 1.5rem;
    background-color: #2f2f2f;
    padding: 1rem;
    border-radius: 8px;
}

.header>div {
    flex: 1 1 200px;
    display: flex;
    flex-direction: column;
}

.header span {
    font-weight: 600;
    margin-bottom: 0.5rem;
    color: #ffffff;
}

.header input {
    padding: 0.5rem;
    border: 1px solid #555;
    border-radius: 6px;
    background-color: #1a1a1a;
    color: #fff;
    font-size: 1rem;
}

.header input::placeholder {
    color: #aaa;
}

button {
    align-self: flex-start;
    padding: 0.6rem 1.2rem;
    font-size: 1rem;
    font-weight: 600;
    background-color: #646cff;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    place-items: center;
}

button:hover {
    background-color: #535bf2;
}

/* #endregion */

.table {
    width: 100%;
    border: 1px solid white;
}

#schedules {
    width: 100%;
    border-collapse: collapse;
}

#schedules th,
#schedules td {
    border: 1px solid white;
    padding: 0;
    border-radius: 3px;
    place-items: center;
    text-align: center;
}
</style>