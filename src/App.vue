<script setup>

import { ref} from 'vue';

// Reactive data for tasks
const taskList = ref(JSON.parse(localStorage.getItem('tasks')) || []);



const isActive = ref(false);
const msg = ref('Show Add Task Form')


function toggleForm() {
    isActive.value = !isActive.value;
    msg.value = isActive.value ? 'Hide Add Task Form' : 'Show Add Task Form';
}

// task store
const task = ref('');




// Add task function
function addTask() {
    task.value.trim().split(/\s+/);
    if (task.value === '' || task.value.length < 3) return alert('Please Enter Task Name & Min 3 Characters')
    taskList.value.push({
        name: task.value.trim(),
        status: false,
    });

    localStorage.setItem('tasks', JSON.stringify(taskList.value))
    task.value = '';
}

const comStyle = {
    backgroundColor: '#88e788',
    border: '1px solid red'
}

// Delete Task Function
function deleteTask(index) {
    alert('Delete this task !')
    taskList.value.splice(index, 1);
    localStorage.setItem('tasks', JSON.stringify(taskList.value))
}



function completeTask(index) {
    taskList.value[index].status = !taskList.value[index].status;
    localStorage.setItem('tasks', JSON.stringify(taskList.value))
}


// localStorage.clear()
let search = ref('');

function getFilteredList() {
    return taskList.value.filter(task => {
        return task.name.toLowerCase().startsWith(search.value.toLowerCase())
    })
};

</script>

<template>
    <div class="container mx-auto ">
        <div class=" h-svh text-center content-center justify-items-center">
            <!-- Create Task -->
            <div class="w-full">
                <h1 class="text-4xl text-slate-500">Task Management App</h1>
                <button class=" px-5 py-1 my-5 rounded text-white text-lg bg-purple-500 hover:bg-slate-500 transition"
                    @click="toggleForm()">{{ msg }}</button>
                <br>
                <div v-show="isActive">
                    <input v-model="task" class="w-6/12 border border-slate-300 px-5 py-2 outline-purple-500"
                        type="text" placeholder="Enter Task Name">
                    <br>
                    <button @click="addTask()"
                        class="bg-purple-500 hover:bg-slate-500 transition px-2 py-2 my-5 rounded text-white text-sm">
                        Add Task </button>
                </div>
            </div>

            <!-- Show Task -->
            <div class=" w-6/12 text-start my-4">
                <input class="px-2 py-2 border border-slate-400 rounded-sm outline-none" v-show="taskList.length !== 0"
                    v-model="search" type="text" placeholder="Search Task">


                <h4 v-show="taskList.length === 0">No tasks available</h4>
            </div>
            <div :style="task.status === true ? comStyle : ''" v-for="(task, index) in getFilteredList()" :key="index"
                class="border rounded-md transition flex items-center justify-between border-slate-300 w-6/12 text-start px-3 mb-3">
                <div>
                    <p>{{ task.name }}</p>
                </div>
                <div>
                    <button @click="completeTask(index)"
                        class="bg-purple-500 hover:bg-slate-500 transition px-2 py-2 my-5 rounded text-white text-sm">
                        {{ task.status === true ? 'Completed' : 'Complete' }} </button>
                    <button @click="deleteTask(index)"
                        class="bg-purple-500 hover:bg-slate-500 transition px-2 py-2 my-5 ml-2 rounded text-white text-sm">
                        Delete </button>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped></style>
