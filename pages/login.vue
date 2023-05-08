<script setup>
const supabase = useSupabaseAuthClient()
const user = useSupabaseUser()
console.log("user", user.value)

if (user.value) {
    await navigateTo("/")
}

const form = reactive({
    email: "cem@cem.com",
    password: "123123"
})

const login = async () => {
    try {
        const { data, error } = await supabase.auth.signInWithPassword({
            email: form.email,
            password: form.password
        })
        console.log("data", data)
        console.log("error", error)
        if (error) {
            throw new Error("auth")
        }
        await navigateTo("/")
    } catch (err) {
        console.log("err", err)
    }
}
</script>

<template>
    <div class="bg-primary h-screen">
        <div class="container mx-auto p-8">
            <div class="w-1/2 mx-auto bg-white rounded-lg shadow-lg p-8">
                <h2 class="text-center text-lg font-bold mb-4">Login</h2>
                <form class="grid grid-cols-1 gap-4" @submit.prevent="login">
                    <div class="form-control">
                        <label class="label" for="email">E-mail</label>
                        <input class="input input-bordered" type="email" id="email" name="email" v-model="form.email" required />
                    </div>
                    <div class="form-control">
                        <label class="label" for="password">Password</label>
                        <input class="input" type="password" id="password" name="password" v-model="form.password" required />
                    </div>
                    <div class="text-center">
                        <button class="btn btn-primary btn-block" type="submit">Login</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>
