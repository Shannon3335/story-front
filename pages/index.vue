<template>
	<NuxtPage class="flex flex-row w-full min-h-screen">
		<UInput v-model="mainCharacter" />
		<UInput v-model="villain" />
		<UInput v-model="setting" />
		<UButton size="xl" @click="generateStory" />
		<UTextarea>{{ story }}</UTextarea>
	</NuxtPage>
</template>

<script setup lang="ts">
const mainCharacter = ref("")
const villain = ref("")
const setting = ref("")
const story = ref("")

const generateStory = async () => {
	try {
		const response = await fetch("http://localhost:8000/generate", {
			method: "POST",
			headers: { "Content-Type": "application/json" },
			body: JSON.stringify({
				mainCharacter: mainCharacter.value,
				villain: villain.value,
				setting: setting.value,
			}),
		})

		const data = await response.json()
		story.value = data.story || "No story returned."
		if (!response.ok) {
			throw new Error(data.error)
		}
	} catch (err) {
		story.value = "Error generating story."
		console.error(err)
	}
}
</script>

<style scoped>
body {
	font-family: "Segoe UI", sans-serif;
}
</style>
