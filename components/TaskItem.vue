<script setup>
const supabase = useSupabaseClient()
const props = defineProps({
    id: {
        type: Number,
        required: true
    },
    task: {
        type: String,
        required: true
    },
    isCompleted: {
        type: Boolean,
        default: () => false
    }
})
const emits = defineEmits(["updateTask"])

const text = ref(props.task)

const updateTask = async () => {
    const { data, error } = await supabase.from("tasks").update({ task: text.value }).eq("id", props.id).select()
    if (error) {
        alert("An error occurred. Please try again later.")
        return
    }
    emits("updateTask", ...data)
}
</script>

<template>
    <li class="container p-8">
        <form class="flex justify-center items-center gap-4" @submit.prevent="updateTask">
            <div class="w-96">
                <div class="relative">
                    <input class="input input-bordered w-full" type="text" v-model="text" :placeholder="text" required />
                    <button class="absolute h-full px-4 rounded-r-lg btn btn-primary">Update</button>
                </div>
            </div>
        </form>
    </li>
</template>
