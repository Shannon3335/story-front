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
	toast.add({
		title: "Success",
		description: "Form has been submitted.",
		color: "success",
	})
	console.log(event.data)
}
</script>
