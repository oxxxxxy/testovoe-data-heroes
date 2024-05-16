<script setup>
	import { ref, onMounted } from 'vue';

	import Filter from './components/Filter.vue';
	import Pagination from './components/Pagination.vue';
	import CharacterTile from './components/CharacterTile.vue';

	const STATE = {};
	STATE.paginationValues = ref({});
	STATE.characters = ref([]);
	
	const ready = ref();
	
	const CACHE = {};
	CACHE.names = {};
	CACHE.characters = {};
	CACHE.pathData = {};



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
		ready.value = false;

		const params = getSearchParams();

		const PATH = 'https://rickandmortyapi.com/api/character/';

		let query = '?' + params;
	
		if(query.length === 1){
			query = '';
		}

		try{
			const link = PATH + query;

			if(!CACHE.pathData[link]){
				const response = await fetch(link);
	
				const json = await response.json();
	

				for(let i = 0; i < json.results.length; i++){
					const character = json.results[i];

					if(!CACHE.characters[character.id]){
						CACHE.characters[character.id] = character;

						for(let k = 0; k < character.episode.length; k++){
							const res = await fetch(character.episode[k]);
							const json = await res.json();
							
							character.episode[k] = {
								name: json.name,
								url: character.episode[k]
							};
						}
					} else {
						json.results[i] = CACHE.characters[character.id];
					}
				}


				CACHE.pathData[link] = {};

				CACHE.pathData[link].pages = json.info.pages;

				STATE.paginationValues.value = {
					pages: json.info.pages,
					selected: STATE.paginationValues.value.selected
				};

				CACHE.pathData[link].results = json.results;
			
				STATE.characters.value = json.results;
			} else {
				STATE.paginationValues.value = {
					pages: CACHE.pathData[link].pages,
					selected: STATE.paginationValues.value.selected
				};

				STATE.characters.value = CACHE.pathData[link].results;
			}

			ready.value = true;
		} catch (e) {
			console.error(e);
			STATE.paginationValues.value = {};
		}
	};

	const clearTempAfterFilterChanges = () => {
		STATE.paginationValues.value = {};
	}

	const filterApply = async obj => (
		STATE.formValues = {...obj},
		clearTempAfterFilterChanges(), 
		await updateCharacters()
	);

	const paginationSelect = async selected => (
		STATE.paginationValues.value = {
			selected, pages: STATE.paginationValues.value.pages
		},
		await updateCharacters()
	);

	onMounted(() => {
		updateCharacters();
	});

</script>

<template>
	<Filter @filter-apply="filterApply"/>
	<div v-if="!ready">Loading... or error :3 Check console and network, sweety! </div>
	<Pagination :active="ready" :paginationValues="STATE.paginationValues" @pagination-select="paginationSelect"/>
	<CharacterTile :active="ready" :characterList="STATE.characters"/>

</template>

<style scoped>

</style>
