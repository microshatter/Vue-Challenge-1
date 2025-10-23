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
        <button class="add-btn" @click="add">Add</button>
    </div>

    <div class="table">
        <table id="schedules" aria-describedby="schedules">
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
                <tr v-for="(i, index) in schedules" :key="index" :class="{ completed: i.is_completed }">
                    <td class="subject">{{ i.subject }}</td>
                    <td class="content">{{ i.content }}</td>
                    <td class="location">{{ i.location }}</td>
                    <td class="completed-col">
                        <input
                            type="checkbox"
                            :id="'checkbox-' + index"
                            v-model="i.is_completed"
                            aria-label="Mark schedule completed"
                        >
                        {{ i.is_completed ? 'Yes' : 'No' }}
                    </td>
                    <td class="action-col">
                        <a href="#" class="delete-btn" @click.prevent="remove_sch(index)">Delete</a>
                    </td>
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

    /* center items vertically so the Add button lines up with inputs */
    align-items: center;
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
/* #endregion */

/* Make the Add button sit to the far right on wide screens and align vertically */
.add-btn {
    align-self: center;
    margin-left: auto; /* pushes button to the rightmost edge */
    padding: 0.6rem 1.2rem;
    font-size: 1rem;
    font-weight: 600;
    background-color: #646cff;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.08s ease;
    flex: 0 0 auto; /* prevent the button from growing/shrinking */
}

.add-btn:hover {
    background-color: #535bf2;
    /* transform: translateY(-1px); */
}

/* On small screens, make the Add button full width and stack beneath inputs */
@media (max-width: 640px) {
    .add-btn {
        margin-left: 0;
        width: 100%;
        align-self: stretch;
        padding: 0.8rem 1rem;
    }
}

/* #endregion */

/* Table container: enables horizontal scroll on small screens */
.table {
    width: 100%;
    border-radius: 8px;
    overflow-x: auto;
    box-shadow: 0 6px 18px rgba(16, 24, 40, 0.06);
    background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    padding: 0.5rem;
}

/* Base table layout */
#schedules {
    width: 100%;
    min-width: 720px; /* ensures columns have room, but table will scroll on small screens */
    border-collapse: collapse;
    font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
}

/* Header styling */
#schedules thead th {
    position: sticky;
    top: 0;
    z-index: 2;
    background: linear-gradient(90deg, #3f51ff, #6f6cff);
    color: #fff;
    padding: 0.75rem 1rem;
    text-align: center;
    font-weight: 700;
    letter-spacing: 0.2px;
    border-bottom: 2px solid rgba(255,255,255,0.06);
}

/* Cell styling */
#schedules th,
#schedules td {
    padding: 0.65rem 0.9rem;
    vertical-align: middle;
    text-align: left;
    border-bottom: 1px solid rgba(255,255,255,0.04);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

/* Column-specific alignment */
#schedules th:nth-child(4),
#schedules td:nth-child(4),
#schedules th:nth-child(5),
#schedules td:nth-child(5) {
    text-align: center;
    white-space: nowrap;
}

/* Zebra striping and hover */
#schedules tbody tr:nth-child(even) {
    background: rgba(99, 102, 241, 0.02);
}

#schedules tbody tr:hover {
    background: rgba(99, 102, 241, 0.06);
    transform: translateZ(0);
}

/* Completed row styling */
.completed td {
    text-decoration: line-through;
    color: #9aa0b4;
    opacity: 0.8;
}

/* Make subject/content columns readable when long */
.subject { max-width: 220px; }
.content { max-width: 360px; }
.location { max-width: 160px; }

/* Checkbox: slightly larger and themed */
input[type="checkbox"] {
    transform: scale(1.15);
    accent-color: #646cff; /* modern browsers support */
    cursor: pointer;
}

/* Action / delete button style */
.delete-btn {
    display: inline-block;
    padding: 0.35rem 0.6rem;
    font-size: 0.9rem;
    color: #ff5252;
    border-radius: 6px;
    text-decoration: none;
    transition: background-color 0.15s ease, color 0.15s ease, transform 0.08s ease;
}

.delete-btn:hover {
    background-color: rgba(255,82,82,0.06);
    color: #ff1f1f;
    transform: translateY(-1px);
}

/* Accessibility: focus states */
.delete-btn:focus,
button:focus,
input:focus {
    outline: 3px solid rgba(100,108,255,0.25);
    outline-offset: 2px;
    border-radius: 6px;
}

/* Responsive tweaks */
@media (max-width: 820px) {
    #schedules {
        min-width: 600px;
    }
    .header {
        gap: 0.6rem;
    }
}
</style>