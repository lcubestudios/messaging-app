<template>
	<form @submit.prevent="userLogin" class="flex flex-col">
		<div class="form-field">
			<input v-model="username" id="username" name="username" class="my-2" type="text" placeholder="Username" required />
		</div>
		<div class="form-field">
			<input v-model="password" id="password" name="password" class="my-2" type="password" placeholder="Password" autocomplete="on" required />
		</div>
		<button type="submit" class="btn bg-btn-bg text-btn-text hover:bg-btn-bg-hover hover:bg-btn-bg-hover:text-btn-text mt-12">LOGIN</button>
		<div class="mt-4 text-center">
			<small>Don't have an account yet? <button class="text-primary" @click="toggleRegForm()">SIGN UP</button></small>
		</div>
	</form>
</template>

<script>
import { ref } from 'vue'
import { useStore } from 'vuex'

export default {
	name: 'LoginForm',
	setup() {
		const store = useStore()
		const username = ref('')
		const password = ref('')

		const toggleRegForm = () => {
			store.dispatch('setAuthForm', 'reg')
		}

		const userLogin = async () => {
			const payload = {
				username: username.value,
				password: password.value
			}

			await store.dispatch('userLogin', payload)
		}

		return {
			toggleRegForm,
			userLogin,
			username,
			password
		}
	},
}
</script>