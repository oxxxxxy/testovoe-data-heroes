<script setup>
	import { ref } from 'vue';

	const emit = defineEmits(['filter-apply']);

	const statusOptions = ref(['Alive', 'Dead', 'unknown']);

	let name = ref('');
	let status = ref('');
	
	const selectStatus = e => {
		status.value = e.target.value;
	}

	const getFilteredCharacters = e => {
		emit(
			'filter-apply',
			{
				name: name.value,
				status: status.value
			}
		);
	}

</script>


<template>
	<form @submit.prevent="getFilteredCharacters">
	  <fieldset>
	    <legend>Filter characters by name, status.</legend>
	    <p>
				<input class="filter-option" placeholder="name" v-model="name">
	    </p>
	    <p>
			<select @change.prevent="selectStatus" class="filter-option">
					<option value=""></option>
					<option v-for="op in statusOptions" :key="op" :value="op">{{op}}</option>
		    </select>
	    </p>
	    <p>
				<button class="filter-option">Apply</button>	
	    </p>
	  </fieldset>
	</form>
</template>


<style scoped>
fieldset{
	border: none;
	display: inline;
}

.filter-option{
	width: 150px;
	height: 35px;
	border: none;
	padding: 0;
	margin: 0;
}
</style>
