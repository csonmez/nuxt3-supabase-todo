<script setup>
const supabase = useSupabaseAuthClient()

const form = reactive({
    name: null,
    surname: null,
    email: null,
    password: null
})

const register = async () => {
    try {
        await supabase.auth.signUp({
            email: form.email,
            password: form.password,
            options: {
                data: {
                    first_name: "deneem"
                }
            }
        })
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
                <h2 class="text-center text-lg font-bold mb-4">Register</h2>
                <form class="grid grid-cols-1 gap-4" @submit.prevent="register">
                    <div class="form-control">
                        <label class="label" for="name">Name</label>
                        <input class="input input-bordered" type="text" id="name" name="name" v-model="form.name" required />
                    </div>
                    <div class="form-control">
                        <label class="label" for="surname">Surname</label>
                        <input class="input input-bordered" type="text" id="surname" name="surname" v-model="form.surname" required />
                    </div>
                    <div class="form-control">
                        <label class="label" for="email">E-mail</label>
                        <input class="input input-bordered" type="email" id="email" name="email" v-model="form.email" required />
                    </div>
                    <div class="form-control">
                        <label class="label" for="password">Password</label>
                        <input class="input" type="password" id="password" name="password" v-model="form.password" required />
                    </div>
                    <div class="text-center">
                        <button class="btn btn-primary btn-block" type="submit">Register</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>
