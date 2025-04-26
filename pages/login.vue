<template>
	<MainContainer>
		<UForm :schema="schema" :state="state" class="space-y-4" @submit="onSubmit">
			<UFormField label="Email" name="email">
				<UInput v-model="state.email" />
			</UFormField>
			<UFormField label="Password" name="password">
				<UInput v-model="state.password" type="password" />
			</UFormField>
			<UButton type="submit">Submit</UButton>
		</UForm>
	</MainContainer>
</template>

<script setup lang="ts">
import type { FormSubmitEvent } from "@nuxt/ui"
import * as z from "zod"

const schema = z.object({
	email: z.string().email(),
	password: z.string().min(5),
})

type Schema = z.output<typeof schema>

const state = reactive<Partial<Schema>>({
	email: undefined,
	password: undefined,
})

const toast = useToast()
async function onSubmit(event: FormSubmitEvent<Schema>) {
	// request to go api here
	const success = await loginUser(event.data)
	// based on success, change toast type

	toast.add(
		success
			? {
					title: "Success",
					description: "Login success",
					color: "success",
			  }
			: {
					title: "Failed",
					description: "Login attept failed",
					color: "error",
			  },
	)
	console.log(event.data)
}

const loginUser = async (userDetails: Schema) => {
	const res = await fetch("http://localhost:8000/login", {
		method: "POST",
		headers: { "Content-type": "application/json" },
		body: JSON.stringify({
			username: userDetails.email,
			password: userDetails.password,
		}),
	})
	const data = await res.json()
	console.log(JSON.stringify(data.Message))
	if (!res.ok) {
		return false
	}
	return true
}
</script>
