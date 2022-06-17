<template>
	<div class="container">
		<h1>To Do List</h1>
		<h4>count: {{ count }}</h4>
		<h4>double count: {{ doubleCount }}</h4>
		<button @click="plusCount">Count + 1</button>
		<ToDoListForm @add-todo="addTodo" />
	</div>
	<div v-if="!todos.length">There is no any Todo.</div>
	<ToDoList :todos="todos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />
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
			todos.value.push(todo);
		};
		const toggleTodo = index => {
			todos.value[index]['completed'] = !todos.value[index]['completed'];
		};
		const count = ref(1);
		const doubleCount = computed(() => {
			return count.value * 2; 
		});
		const plusCount = () => {
			count.value++;
		};

		return {
			todos,
			addTodo,
			toggleTodo,
			deleteTodo,
			count,
			doubleCount,
			plusCount,
		};
	},
};
</script>

<style></style>
