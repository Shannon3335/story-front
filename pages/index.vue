<template>
	<MainContainer>
		<UInput v-model="mainCharacter" placeholder="Enter your hero" />
		<UInput v-model="villain" placeholder="Enter the villain" />
		<UInput v-model="setting" placeholder="Enter the setting" />
		<UButton size="xl" @click="generateStory">Generate Story</UButton>
		<UTextarea v-model="story" :rows="10" readonly />
	</MainContainer>
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
		console.log(JSON.stringify(data))
		story.value = data.Message || "No story returned."
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
