<template>
	<div class="container">
		<h1>To Do List</h1>
		<input type="text" placeholder="Search" v-model="searchText" />
		<hr />
		<ToDoListForm @add-todo="addTodo" />
	</div>
	<div v-show="!todos.length">There is no any Todo.</div>
	<div v-show="!filteredTodos.length">There is nothing to display.</div>
	<ToDoList :todos="filteredTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />
</template>

<script>
import { ref, computed } from 'vue';
import ToDoListForm from './components/ToDoListForm.vue';
import ToDoList from './components/ToDoList.vue';

export default {
	components: {
		ToDoListForm,
		ToDoList,
	},
	setup() {
		const todos = ref([]);
		const deleteTodo = index => {
			todos.value.splice(index, 1);
		};
		const addTodo = todo => {
			try {
				const xmlHttp = new XMLHttpRequest();
				xmlHttp.open('POST', 'http://localhost:3000/todos');
				xmlHttp.setRequestHeader('content-type', 'application/json');
				xmlHttp.send(JSON.stringify({ id: Date.now(), subject: todo.subject, completed: todo.completed }));
				todos.value.push(todo);
			} catch (error) {
				console.log(error);
			}
		};
		const toggleTodo = index => {
			todos.value[index]['completed'] = !todos.value[index]['completed'];
		};
		const searchText = ref('');
		const filteredTodos = computed(() => {
			if (searchText.value) return todos.value.filter(todo => todo.subject.includes(searchText.value));
			return todos.value;
		});
		return {
			todos,
			addTodo,
			toggleTodo,
			deleteTodo,
			searchText,
			filteredTodos,
		};
	},
};
</script>

<style></style>
