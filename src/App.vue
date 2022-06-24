<template>
	<h1 class="h1ToDoListTitle">To Do List Practice</h1>
	<SearchToDoVue @search-keyword="searchToDo" />
	<AddToDoVue @add-keyword="addToDo" />
	<ToDoItemsVue :toDos="toDos" @change-completed="changeCompleted" @delete-to-do="deleteToDo" />
	<ShowToDoListButtonsVue :totalToDos="totalToDos" :toDoItemLimitCount="toDoItemLimitCount" @current-page="showToDoList" />
</template>

<script>
import { ref, watch } from 'vue';
import SearchToDoVue from './components/SearchToDo.vue';
import AddToDoVue from './components/AddToDo.vue';
import ShowToDoListButtonsVue from './components/ShowToDoListButtons.vue';
import ToDoItemsVue from './components/ToDoItems.vue';

export default {
	components: {
		SearchToDoVue,
		AddToDoVue,
		ShowToDoListButtonsVue,
		ToDoItemsVue,
	},
	setup() {
		const totalToDos = ref([]);
		watch(totalToDos, () => {
			console.log(totalToDos.value);
		});
		const toDos = ref([]);
		const toDoItemLimitCount = 10;
		const init = async () => {
			await fetch('http://localhost:3000/toDoList?_sort=id&_order=desc')
				.then(res => res.json())
				.then(res => (totalToDos.value = [...res]))
				.then(() => (toDos.value = [...totalToDos.value].splice(0, toDoItemLimitCount)))
				.catch(err => console.log(err));
		};
		const searchToDo = async searchKeyword => {
			await fetch(`http://localhost:3000/toDoList?_sort=id&_order=desc&subject_like=${searchKeyword}`)
				.then(res => res.json())
				.then(res => (totalToDos.value = [...res]))
				.then(() => (toDos.value = [...totalToDos.value].splice(0, toDoItemLimitCount)))
				.catch(err => console.log(err));
		};
		const addToDo = async toDoKeyword => {
			await fetch('http://localhost:3000/toDoList', {
				method: 'POST',
				headers: { 'content-type': 'application/json' },
				body: JSON.stringify({ id: Date.now(), subject: toDoKeyword, completed: false }),
			})
				.then(() => init())
				.catch(err => console.log(err));
		};
		const showToDoList = currentPageIndex => {
			toDos.value = [...totalToDos.value].splice(currentPageIndex * toDoItemLimitCount, currentPageIndex * toDoItemLimitCount ? currentPageIndex * toDoItemLimitCount : toDoItemLimitCount);
		};
		const changeCompleted = (id, completed) => {
			console.log(id, completed);
		};
		const deleteToDo = id => {
			// RESTful API
			// totalToDos 초기화
			// toDos 초기화
			console.log(id);
		};

		init();

		return {
			totalToDos,
			toDos,
			toDoItemLimitCount,
			searchToDo,
			addToDo,
			showToDoList,
			changeCompleted,
			deleteToDo,
		};
	},
};
</script>

<style>
.h1ToDoListTitle {
	font-family: 'Times New Roman', Times, serif;
	font-size: 5vw;
	margin-bottom: 3vw;
}
</style>
