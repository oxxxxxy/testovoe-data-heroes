<script setup>
	import { ref, watch } from 'vue';

	const emit = defineEmits(['pagination-select']);
	const props = defineProps(['paginationValues']);

	let pageLength = ref(0);
	let currentPageRange = ref([]);
	let ready = true;

	const buttonClick = id => {
		if(!ready){
			return;
		}
		
		emit('pagination-select', id);

		ready = false;
	}

	const calcPaginationRange = (countOfPages, selectedPage) => {
		const buttonViewingLimit = 5;// tolko nechetnie, pj
		const sideInteger = Math.floor(buttonViewingLimit / 2);

		const calc = (init, limit) => {
			const pageRange = [];

			for(let i = 0; i < limit; i++){
				pageRange.push({id: i + init});
			}

			const index = pageRange.findIndex(e => e.id === selectedPage);
			pageRange[index].selected = true;

			return pageRange;
		}
		
		if(countOfPages <= buttonViewingLimit){
			return calc(1, countOfPages);
		}

		const diff = selectedPage - sideInteger;

		if(diff <= 0){
			return calc(1, buttonViewingLimit);
		}

		const sum = selectedPage + sideInteger;

		if(diff > 0 && sum < countOfPages + 1){
			return calc(diff, buttonViewingLimit);
		}

		if(sum >= countOfPages + 1){
			return calc(diff - sum + countOfPages, buttonViewingLimit);
		}
	};

	const recieveNewPaginationValues = obj => {
		const {pages, selected} = obj;

		if(pages && !selected){
			currentPageRange.value = calcPaginationRange(pages, 1);
			pageLength.value = pages;
		} else if (pages && selected){
			currentPageRange.value = calcPaginationRange(pages, selected);
		} else {
			pageLength.value = 0;
			currentPageRange.value = [];
		}
		
		ready = true;
	};

	watch(props.paginationValues, recieveNewPaginationValues);
	
</script>

<template>
	<div v-if="pageLength">
		<button 
			 @click.prevent="buttonClick(page.id)" 
			v-for="page in currentPageRange" :key="page.id"
			:class="page.selected ? 'selected' : ''"
		>
			{{page.id}}
		</button>
	</div>
	<div v-else></div>
</template>

<style scoped>
div{
	height: 50px;
}

.selected{
	background-color: rgba(201, 10, 100, 0.4);
}
</style>
