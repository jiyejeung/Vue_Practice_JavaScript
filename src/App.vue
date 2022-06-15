<template>
	<div class="divNameContainer">{{ name }}</div>
</template>
<script>
export default {
	setup() {
		const name = 'Yejeung';
		return { name };
	},
};
</script>
<style>
.divNameContainer {
	color: coral;
}
</style>
