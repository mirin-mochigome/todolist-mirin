<script setup lang="ts">
import { computed, ref } from 'vue'

interface Task {
    name: string
    isfinished: boolean
}

const tasks = ref<Task[]>([])

const finishedtasks = computed(() => tasks.value.filter((task) => task.isfinished))

const unfinishedtasks = computed(() => tasks.value.filter((task) => !task.isfinished))

const newtaskname = ref('')

const addtask = () => {

    if (newtaskname.value === '') {
        alert('タスク名を入力してください')
        return
    }
    if (tasks.value.some((task) => task.name === newtaskname.value)) {
        alert('もうある')
        return
    }
    tasks.value.push({
        name: newtaskname.value,
        isfinished: false
    })
    newtaskname.value = ''
}
const finishtask = (taskname: string) => {

    tasks.value = tasks.value.map((task) => {
        if (task.name === taskname) {
            return {
                ...task,
                isfinished: true
            }
        }
        return task
    })
}
const re = (taskname: string) => {
    tasks.value = tasks.value.map((task) => {
        if (task.name === taskname) {
            return {
                ...task,
                isfinished: false
            }
        }
        return task
    })
}
const deletetask = (task: Task) => {
    const del = tasks.value.indexOf(task)
        tasks.value.splice(del, 1)
}
</script>

<template>
    <div>
        <h1>TodoList</h1>
        <h3>終わったやつ</h3>
        <ul>
            <li v-for="task in finishedtasks" :key="task.name">
                <div>{{ task.name }}</div>
                <button @click=re(task.name)>復元</button>
                <button @click=deletetask(task)>削除</button>
            </li>
        </ul><br>
        <h3>未完タスク</h3>
        <ul>
            <li v-for="task in unfinishedtasks" :key="task.name">
                <div>{{ task.name }}</div>
                <div>
                    <button @click="finishtask(task.name)">完了</button>
                </div>
            </li>
        </ul>
        <div>
            <label><br>
                名前
                <input v-model="newtaskname" type="text" />
            </label>
            <button @click="addtask">追加</button>
        </div>
    </div>
</template>