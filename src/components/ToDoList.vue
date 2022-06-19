<template>
	<div v-for="(todo, index) in todos" :key="todo.id">
		<div>
			<input type="checkbox" :value="todo.completed" @change="toggleTodo(index)" />
			<label :class="{ todo: todo.completed }">{{ todo.subject }}</label>
			<button @click="deleteTodo(index)">Delete</button>
		</div>
	</div>
</template>

<script>
// import { watchEffect } from 'vue';

export default {
	props: { todos: { type: Array, required: true } },
	emits: ['toggle-todo', 'delete-todo'],
	setup(props, context) {
		// watchEffect(() => {
		// 	console.log(props.todos.length);
		// });
		const toggleTodo = index => {
			context.emit('toggle-todo', index);
		};
		const deleteTodo = index => {
			context.emit('delete-todo', index);
		};
		return { toggleTodo, deleteTodo };
	},
};
</script>

<style></style>
