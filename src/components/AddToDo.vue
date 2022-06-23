<template>
	<form class="formAddToDoContainer" @submit.prevent="submitToAdd">
		<input class="inputAddToDo" type="text" placeholder="Add..." v-model="toDoKeyword" />
		<button class="buttonAddToDo" type="submit">ADD</button>
	</form>
</template>

<script>
import { ref } from 'vue';

export default {
	setup(_, context) {
		const toDoKeyword = ref('');
		const submitToAdd = () => {
			context.emit('add-keyword', toDoKeyword.value);
		};
		return { toDoKeyword, submitToAdd };
	},
};
</script>

<style>
.formAddToDoContainer {
	width: 100%;
	display: flex;
	justify-content: center;
	align-items: center;
	margin-bottom: 3vw;
}
.inputAddToDo {
	width: 40%;
	font-size: 2vw;
	margin-right: 1vw;
	padding-left: 1vw;
}
.buttonAddToDo {
	width: 15%;
	font-size: 2vw;
	cursor: pointer;
}
</style>
