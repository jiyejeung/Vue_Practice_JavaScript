<template>
	<div class="divShowToDoListWrapperContainer" v-if="totalToDos.length">
		<div class="divPrevShow" @click="showPrev">Prev</div>
		<div class="divPageContainer" v-for="(toDo, index) in totalPage" :key="toDo">
			<div @click="showCurrent(index + 1)">{{ index + 1 }}</div>
		</div>
		<div class="divNextShow" @click="showNext">Next</div>
	</div>
</template>

<script>
import { ref, watch } from '@vue/runtime-core';

export default {
	props: {
		totalToDos: {
			type: Array,
			required: true,
		},
		toDoItemLimitCount: {
			type: Number,
			required: true,
		},
	},
	emits: ['current-page'],
	setup(props, context) {
		const currentPage = ref(1);
		const totalPage = ref(0);
		watch(props, () => {
			totalPage.value = Math.ceil(props.totalToDos.length / props.toDoItemLimitCount);
		});
		watch(currentPage, () => void context.emit('current-page', currentPage.value));

		const showPrev = () => void (currentPage.value = currentPage.value - 1 ? currentPage.value - 1 : 1);
		const showCurrent = currentPageNum => void (currentPage.value = currentPageNum);
		const showNext = () => void (currentPage.value = currentPage.value + 1 > totalPage.value ? currentPage.value : currentPage.value + 1);

		return {
			currentPage,
			totalPage,
			showPrev,
			showCurrent,
			showNext,
		};
	},
};
</script>

<style>
div {
	font-size: 1.5vw;
}
.divShowToDoListWrapperContainer {
	margin-top: 4vw;
	display: flex;
	justify-content: center;
	align-items: center;
	width: 100%;
}
.divPageContainer div {
	margin-right: 0.5vw;
	width: 2vw;
	height: 2vw;
	border: 1px solid #000;
	line-height: 2vw;
	text-align: center;
	cursor: pointer;
}
.divPrevShow {
	width: 4vw;
	height: 2vw;
	border: 1px solid #000;
	line-height: 2vw;
	text-align: center;
	margin-right: 1vw;
	cursor: pointer;
}
.divNextShow {
	width: 4vw;
	height: 2vw;
	border: 1px solid #000;
	line-height: 2vw;
	text-align: center;
	margin-left: 0.5vw;
	cursor: pointer;
}
</style>
