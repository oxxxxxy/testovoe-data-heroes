<script setup>
	import { ref } from 'vue';

	import Filter from './components/Filter.vue';
	import Pagination from './components/Pagination.vue';

	const STATE = {};
	STATE.paginationValues = ref({});

	function log(...args){console.log('log', ...args)}

	const getSearchParams = () => {
		let obj = {};

		const form = STATE.formValues;
		if(form){
			for(const k in form){
				if(form[k]){
					obj[k] = form[k];
				}
			};
		}
		
		if(STATE.paginationValues.value.selected){
			obj.page = STATE.paginationValues.value.selected;
		}

		return new URLSearchParams(obj);
	}

	const updateCharacters = async () => {
		const params = getSearchParams();

		

		log('upCha', STATE, params + '')
	};

	const clearTempAfterFilterChanges = () => {
		STATE.paginationValues.value = {};
	}

	const updateAfterChanges = () => {
		//hashtag navigation

		updateCharacters();
	}

	const filterApply = obj => (
		STATE.formValues = {...obj},
		clearTempAfterFilterChanges(), //hueva
		updateAfterChanges()
	);

setTimeout(()=>{STATE.paginationValues.value = {pages: 7}}, 1000);

	const paginationSelect = selected => (
		STATE.paginationValues.value = {
			selected, pages: STATE.paginationValues.value.pages
		},
		updateAfterChanges()
	);

</script>

<template>
	<Filter @filter-apply="filterApply"/>
	<Pagination :paginationValues="STATE.paginationValues" @pagination-select="paginationSelect"/>

</template>

<style scoped>

</style>
