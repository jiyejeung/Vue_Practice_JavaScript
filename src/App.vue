<template>
	<div class="divExample01">{{ nation }}</div>
	<div class="divExample02">{{ objExample[0].name }}</div>
	<button class="buttonExample" v-on:click="clickExample()">CLICK</button>
</template>
<script>
import { ref, reactive } from 'vue';

export default {
	setup() {
		const nation = ref('Korea');
		const objExample = reactive([
			{ id: 1, name: 'yejeung' },
			{ id: 2, name: 'yein' },
		]);
		const reactiveExample = reactive('Yejeung');
		const clickExample = () => void ((nation.value = 'America'), (objExample[0].name = 'yerim'));
		return { nation, objExample, clickExample, reactiveExample };
	},
};
</script>
<style></style>
