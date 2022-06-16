<template>
	<h1 class="h1TItle">To-Do List</h1>
	<form class="formContainer" @submit.prevent="confirmEmpty(), addToDoList()">
		<input class="inputToDoTitle" type="text" placeholder="Add To Do List" v-model="toDoList" />
		<button class="buttonAddToDoList" type="submit">ADD</button>
	</form>
	<br />
	<div class="divToggleContainer" v-show="handlerDivToggleContainer">This field must be fulfilled.</div>
	<div class="divToDoListsContainer" v-for="toDo in toDoLists" :key="toDo.id">
		<div class="divToDoListContainer">{{ toDo.toDoList }}</div>
	</div>
</template>
<script>
import { ref } from 'vue';

export default {
	setup() {
		const toDoList = ref('');
		const toDoLists = ref([]);
		const uniqueID = ref(1);
		const handlerDivToggleContainer = ref(false);
		const confirmEmpty = () => {
			toDoList.value === '' && (handlerDivToggleContainer.value = true);
			toDoList.value !== '' && (handlerDivToggleContainer.value = false);
		};
		const addToDoList = () => void (!handlerDivToggleContainer.value && toDoLists.value.push({ id: uniqueID.value, toDoList: toDoList.value }) && uniqueID.value++);

		return {
			toDoList,
			toDoLists,
			addToDoList,
			handlerDivToggleContainer,
			confirmEmpty,
		};
	},
};
</script>
<style>
.divToDoListContainer {
	border: 1px solid black;
	margin-top: 10px;
	font-family: 'Times New Roman', Times, serif;
}
</style>
