<template>
	<h1>To-Do List</h1>
	<input type="text" v-model="modelExample" />
	<button class="buttonExample" @click="clickExample()">CLICK</button>
	<div>{{ modelExample }}</div>
</template>
<script>
import { ref } from 'vue';

export default {
	setup() {
		const modelExample = ref('Yejeung');
		const clickExample = () => void console.log(modelExample.value);
		return { modelExample, clickExample };
	},
};
</script>
<style></style>
