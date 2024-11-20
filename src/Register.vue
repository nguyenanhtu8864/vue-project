<template>
    <div class="h-screen flex justify-center items-center bg-blue-100">
        <div class=" max-w-md w-full p-4 bg-blue-300 rounded-md">
            <h2 class="text-center text-2xl">Register</h2>
            <form @submit.prevent="submit">
                <div class="mt-4">
                    <label for="username" class="block">Username</label>
                    <input id="username" v-model="form.username" type="text" class="w-full rounded" required>
                </div>

                <div class="mt-4">
                    <label for="name" class="block">Name</label>
                    <input id="name" v-model="form.name" type="text" class="w-full rounded" required>
                </div>

                <div class="mt-4">
                    <label for="password" class=" block">Password</label>
                    <input id="password" v-model="form.password" type="password" class="w-full rounded" required>
                </div>

                <div class="mt-4">
                    <label for="reenterPassword" class=" block">Re-enter Password</label>
                    <input id="reenterPassword" v-model="form.reenterPassword" type="password" class="w-full rounded"
                        required>
                </div>
                <p v-if="err" class=" text-red-500">{{ err }}</p>
                <button type="submit" class="w-full mt-4 bg-blue-500 rounded" :disabled="isLoading">{{ isLoading ?
                    "Submiting..." : "Submit" }}</button>
            </form>
        </div>

    </div>

</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
const API_URL = import.meta.env.VITE_API_URL;
const apiClient = axios.create({
    baseURL: API_URL,
    timeout: 5000
})
const initForm = {
    username: '',
    name: '',
    password: '',
    reenterPassword: ''
}
const form = ref({ ...initForm })
const err = ref('')
const isLoading = ref(false)

async function submit() {
    if (isLoading.value || !validateForm()) return
    isLoading.value = true
    try {
        const { data } = await apiClient.post("/users/register", { "username": form.value.username, "name": form.value.name, "password": form.value.password })
        alert(data.message)
        form.value = { ...initForm };
        err.value = "";
    } catch (error) {
        console.log("error", error.response)
        err.value = error.response?.data?.detail || error.message
    } finally {
        isLoading.value = false
    }
}
function validateForm() {
    if (form.value.password !== form.value.reenterPassword) {
        err.value = "Passwords do not match."
        return false
    }
    if (form.value.password.length < 8) {
        err.value = "Password must be at least 8 characters long.";
        return false;
    }
    if (!/[!@#$%^&*()\-_=+]/.test(form.value.password)) {
        err.value = "Password must contain at least one special character.";
        return false;
    }
    if (!/\d/.test(form.value.password)) {
        err.value = "Password must contain at least one number.";
        return false;
    }
    if (!/[A-Z]/.test(form.value.password)) {
        err.value = "Password must contain at least one uppercase letter.";
        return false;
    }
    err.value = ''
    return true
}


</script>