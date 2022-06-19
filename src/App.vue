<template>
	<div class="container">
		<h1>To Do List</h1>
		<input type="text" placeholder="Search" v-model="searchText" @keyup.enter="searchTodo" />
		<hr />
		<ToDoListForm @add-todo="addTodo" />
	</div>
	<div v-show="!todos.length">There is nothing to display.</div>
	<ToDoList :todos="todos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />
	<hr />
	<ul class="paginationContainer">
		<li @click="previousPage">Previous</li>
		<li v-for="page in currentPage" :key="page" :class="numPage === page ? 'clicked' : ''" @click="initTodos(page)">{{ page }}</li>
		<li @click="nextPage">Next</li>
	</ul>
</template>

<script>
import { ref, computed, watch /* watchEffect, reactive */ } from 'vue';
import ToDoListForm from './components/ToDoListForm.vue';
import ToDoList from './components/ToDoList.vue';

export default {
	components: {
		ToDoListForm,
		ToDoList,
	},
	setup() {
		// const fetch = require('node-fetch'); Node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지
		const todos = ref([]);
		const totalPage = ref(0);
		const limit = 5;
		const numPage = ref(1);
		const searchText = ref('');
		const currentPage = computed(() => Math.ceil(totalPage.value / limit));

		// watch(numPage, (newV, prevV) => {
		// 	console.log(newV, prevV);
		// });

		// const example01 = reactive({ exampleKey: 1 });
		// watchEffect(() => {
		// 	console.log(example01.exampleKey);
		// });
		// example01.exampleKey = 3;

		const initTodos = async (page = numPage.value) => {
			numPage.value = page;
			// pagination
			await fetch(`http://localhost:3000/todos?subject_like=${searchText.value}`)
				.then(res => res.json())
				.then(res => (totalPage.value = res.length))
				.catch(err => console.log(err));

			await fetch(`http://localhost:3000/todos?_sort=id&_order=desc&subject_like=${searchText.value}&_page=${page}&_limit=${limit}`)
				.then(res => res.json())
				.then(res => (todos.value = res))
				.catch(err => console.log(err));
		};
		initTodos();
		const previousPage = async () => {
			numPage.value = numPage.value - 1 === 0 ? 1 : numPage.value - 1;
			await fetch(`http://localhost:3000/todos?_page=${numPage.value}&_limit=${limit}`)
				.then(res => res.json())
				.then(res => (todos.value = res))
				.catch(err => console.log(err));
		};
		const nextPage = async () => {
			numPage.value = numPage.value === currentPage.value ? numPage.value : numPage.value + 1;
			await fetch(`http://localhost:3000/todos?_page=${numPage.value}&_limit=${limit}`)
				.then(res => res.json())
				.then(res => (todos.value = res))
				.catch(err => console.log(err));
		};
		const deleteTodo = async index => {
			const id = todos.value[index]['id'];
			await fetch(`http://localhost:3000/todos/${id}`, {
				method: 'DELETE',
			})
				.then(res => {
					if (!res.ok) throw new Error(res.statusText);
					return res.json();
				})
				.then(() => initTodos())
				.catch(err => console.log(err));
		};
		const addTodo = async todo => {
			// fetch
			await fetch('http://localhost:3000/todos', {
				method: 'POST',
				headers: { 'content-type': 'application/json' },
				body: JSON.stringify({ id: todo.id, subject: todo.subject, completed: todo.completed }),
			})
				.then(res => {
					if (!res.ok) throw new Error(res.statusText);
					return res.json();
				})
				.then(() => initTodos())
				.catch(err => console.log(err));
			//
			// Promise
			// new Promise(resolve => {
			// 	const xmlHttp = new XMLHttpRequest();
			// 	xmlHttp.open('POST', 'http://localhost:3000/todos');
			// 	xmlHttp.setRequestHeader('content-type', 'application/json');
			// 	xmlHttp.send(JSON.stringify({ id: Date.now(), subject: todo.subject, completed: todo.completed }));
			// 	resolve(todo);
			// })
			// 	.then(res => todos.value.push(res))
			// 	.catch(err => console.log(err));
			//
			// XMLHttpRequest
			// try {
			// 	const xmlHttp = new XMLHttpRequest();
			// 	xmlHttp.open('POST', 'http://localhost:3000/todos');
			// 	xmlHttp.setRequestHeader('content-type', 'application/json');
			// 	xmlHttp.send(JSON.stringify({ id: Date.now(), subject: todo.subject, completed: todo.completed }));
			// 	todos.value.push(todo)
			// } catch (err) {
			// 	console.log(err);
			// }
		};
		const toggleTodo = async index => {
			const id = todos.value[index]['id'];
			await fetch(`http://localhost:3000/todos/${id}`, {
				method: 'PATCH',
				headers: { 'content-type': 'application/json' },
				body: JSON.stringify({ completed: !todos.value[index]['completed'] }),
			})
				.then(res => res.json())
				.then(res => (todos.value[index]['completed'] = res['completed']))
				.catch(err => console.log(err));
		};
		let timeout = null;
		const searchTodo = () => {
			clearTimeout(timeout);
			initTodos();
		};
		watch(searchText, () => {
			clearTimeout(timeout);
			timeout = setTimeout(() => {
				initTodos();
			}, 1000);
		});
		// const filteredTodos = computed(() => {
		// 	if (searchText.value) return todos.value.filter(todo => todo.subject.includes(searchText.value));
		// 	return todos.value;
		// });
		return {
			todos,
			currentPage,
			numPage,
			previousPage,
			nextPage,
			initTodos,
			addTodo,
			toggleTodo,
			deleteTodo,
			searchText,
			searchTodo,
			// filteredTodos,
		};
	},
};
</script>

<style>
.paginationContainer {
	display: flex;
	list-style: none;
	justify-content: center;
	align-items: center;
}
.paginationContainer li {
	margin: 10px;
	cursor: pointer;
}
.clicked {
	color: coral;
	font-weight: bold;
}
</style>
