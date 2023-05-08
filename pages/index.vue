<script setup>
import TaskItem from "~/components/TaskItem.vue"

const supabase = useSupabaseClient()
const user = useSupabaseUser()

if (!user.value) {
    await navigateTo("/login")
}

const task = ref(null)

const { data: tasks } = await useAsyncData("tasks", async () => {
    const { data, error } = await supabase.from("tasks").select().order("created_at", { ascending: false })
    console.log("data", data)
    return data
})

const addTask = async () => {
    const { data, error } = await supabase.from("tasks").insert({ owner: user.value.id, task: task.value }).select()
    if (error) {
        alert("An error occurred. Please try again later.")
        return
    }
    tasks.value.unshift(...data)
}

const updateTask = (data) => {
    const item = tasks.value.find((i) => i.id === data.id)
    Object.assign(item, data)
}

const logout = async () => {
    await supabase.auth.signOut()
    await navigateTo("/login")
}
</script>

<template>
    <div class="grid grid-cols-1">
        <div class="flex justify-center">
            <div class="card w-96 bg-primary shadow-xl">
                <div class="card-body">
                    <h2 class="card-title">Add Task</h2>
                    <div class="card-actions justify-end">
                        <form @submit.prevent="addTask">
                            <input type="text" placeholder="Task" class="input input-bordered w-full max-w-xs" v-model="task" required />
                            <button class="btn btn-primary" type="submit">Add Task</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <div class="flex justify-center">
            <ul v-if="tasks && tasks.length">
                <TaskItem v-for="t in tasks" :key="'t' + t.id" :id="t.id" :task="t.task" :is-completed="t.isCompleted" @update-task="updateTask" />
            </ul>
        </div>
    </div>
</template>
