<template>
    <div class="h-screen flex justify-center items-center bg-gray-50">
        <div class="max-w-md w-full p-8 bg-white rounded-lg shadow-xl">
            <h2 class="text-center text-3xl font-extrabold text-blue-600">Create an Account</h2>
            <p class="text-center text-gray-500 text-sm mt-2">Please fill out the form below to register</p>
            <form @submit.prevent="submit" class="space-y-6 mt-6">
                <div>
                    <label for="username" class="block text-gray-600 font-semibold">Username</label>
                    <input id="username" v-model="form.username" type="text"
                        class="w-full rounded-lg border border-gray-300 p-3 focus:ring-2 focus:ring-blue-400 focus:outline-none"
                        placeholder="Enter your username" required>
                </div>

                <div>
                    <label for="name" class="block text-gray-600 font-semibold">Name</label>
                    <input id="name" v-model="form.name" type="text"
                        class="w-full rounded-lg border border-gray-300 p-3 focus:ring-2 focus:ring-blue-400 focus:outline-none"
                        placeholder="Enter your name" required>
                </div>

                <div>
                    <label for="password" class="block text-gray-600 font-semibold">Password</label>
                    <input id="password" v-model="form.password" type="password"
                        class="w-full rounded-lg border border-gray-300 p-3 focus:ring-2 focus:ring-blue-400 focus:outline-none"
                        placeholder="Enter your password" required>
                </div>

                <div>
                    <label for="reenterPassword" class="block text-gray-600 font-semibold">Re-enter Password</label>
                    <input id="reenterPassword" v-model="form.reenterPassword" type="password"
                        class="w-full rounded-lg border border-gray-300 p-3 focus:ring-2 focus:ring-blue-400 focus:outline-none"
                        placeholder="Confirm your password" required>
                </div>

                <p v-if="err" class="text-red-500 text-sm mt-2">{{ err }}</p>

                <button type="submit"
                    class="w-full bg-blue-600 text-white font-semibold rounded-lg py-3 hover:bg-blue-700 transition-all duration-300 disabled:opacity-50 disabled:cursor-not-allowed"
                    :disabled="isLoading">
                    {{ isLoading ? "Submitting..." : "Submit" }}
                </button>
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