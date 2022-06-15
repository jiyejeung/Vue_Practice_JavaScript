<template>
	<div class="divNameContainer">{{ greeting(name) }} {{ age }}</div>
	<button class="buttonExample" v-on:click="clickExample()">CLICK</button>
</template>
<script>
export default {
	setup() {
		const name = 'Yejeung';
		const age = 27;
		const greeting = nameArg => 'Hello ' + nameArg;

		const clickExample = () => void console.log(greeting(name));
		return { name, age, greeting, clickExample };
	},
};
</script>
<style>
.divNameContainer {
	color: coral;
}
</style>
