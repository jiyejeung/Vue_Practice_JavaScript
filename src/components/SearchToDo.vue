<template>
	<form class="formSearchToDoContainer" @submit.prevent="submitToSearch">
		<input class="inputSearchToDo" type="text" placeholder="Search..." v-model="searchKeyword" @keydown="submitToSearch" />
		<button class="buttonSearchToDo" type="submit">SEARCH</button>
	</form>
</template>

<script>
import { ref, watch } from 'vue';
export default {
	emits: ['search-keyword'],
	setup(_, context) {
		const searchKeyword = ref('');
		watch(searchKeyword, () => {
			submitToSearch();
		});
		const submitToSearch = () => {
			context.emit('search-keyword', searchKeyword.value);
		};
		return { searchKeyword, submitToSearch };
	},
};
</script>

<style>
.formSearchToDoContainer {
	width: 100%;
	display: flex;
	justify-content: center;
	align-items: center;
	margin-bottom: 3vw;
}
.inputSearchToDo {
	width: 40%;
	font-size: 2vw;
	margin-right: 1vw;
	padding-left: 1vw;
}
.buttonSearchToDo {
	width: 15%;
	font-size: 2vw;
	cursor: pointer;
}
</style>
