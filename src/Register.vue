<template>
    <div class="h-screen flex justify-center items-center bg-blue-100">
        <div class=" max-w-md w-full p-4 bg-blue-300 rounded-md">
            <h2 class="text-center text-2xl">Register</h2>
            <form @submit.prevent="submit">
                <div class="mt-4">
                    <label for="username" class="block">Username</label>
                    <input v-model="form.username" type="text" class="w-full rounded" required>
                </div>

                <div class="mt-4">
                    <label for="name" class="block">Name</label>
                    <input v-model="form.name" type="text" class="w-full rounded" required>
                </div>

                <div class="mt-4">
                    <label for="password" class=" block">Password</label>
                    <input v-model="form.password" type="password" class="w-full rounded" required>
                </div>

                <div class="mt-4">
                    <label for="reenterPassword" class=" block">Re-enter Password</label>
                    <input v-model="form.reenterPassword" type="password" class="w-full rounded" required>
                </div>
                <button type="submit" class="w-full mt-4 bg-blue-500 rounded">Submit</button>
            </form>
        </div>

    </div>

</template>

<script setup>
import { ref } from 'vue'
const initForm = {
    username: '',
    name: '',
    password: '',
    reenterPassword: ''
}
const form = ref({ ...initForm })
async function submit() {
    console.log(form.value)
}
</script>