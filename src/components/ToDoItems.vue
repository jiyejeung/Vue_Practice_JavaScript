<template>
	<ul class="ulToDoItemsContainer" v-if="toDos.length">
		<li class="liToDoItem" v-for="toDo in toDos" :key="toDo.id">
			<input type="checkbox" v-model="toDo.completed" @change="changeCompleted(toDo.id, toDo.completed)" />
			<span>To Do Subject: {{ toDo.subject }}</span>
			<button class="buttonDeleteToDoItem" @click="deleteToDo(toDo.id)">DELETE</button>
		</li>
	</ul>
	<span class="spanEmptyToDos" v-else>There is no To Do List.</span>
</template>

<script>
export default {
	props: { toDos: { type: Array, required: true } },
	emits: ['change-completed', 'delete-to-do'],
	setup(_, context) {
		const changeCompleted = (id, completed) => void context.emit('change-completed', id, completed);
		const deleteToDo = id => void context.emit('delete-to-do', id);

		return {
			changeCompleted,
			deleteToDo,
		};
	},
};
</script>

<style>
ul.ulToDoItemsContainer {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	width: 53%;
}
li {
	width: 100%;
	display: flex;
	padding: 0 2vw;
	justify-content: space-between;
	align-items: center;
	height: 3vw;
	border: 1px solid #000;
	list-style: none;
	margin-bottom: 0.5vw;
}
button {
	width: 20%;
	font-size: 1.5vw;
	cursor: pointer;
}
</style>
