<template>
	<form @submit.prevent="onSubmit">
		<input type="text" v-model="todo" placeholder="Type new to-do." />
		<button type="submit">Add</button>
		<div v-show="hasError">This field cannot be empty.</div>
	</form>
</template>

<script>
import { ref } from 'vue';
export default {
	emits: ['add-todo'],
	setup(props, context) {
		const todo = ref('');
		const hasError = ref(false);
		const onSubmit = () => {
			if (todo.value === '') {
				hasError.value = true;
			} else {
				context.emit('add-todo', { id: Date.now(), subject: todo.value, completed: false }); // 부모 컴포넌트로 이벤트를 보낼 수 있다.
				hasError.value = false;
				todo.value = '';
			}
		};
		return {
			todo,
			hasError,
			onSubmit,
		};
	},
};
</script>

<style></style>
