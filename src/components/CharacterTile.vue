<script setup>

	import { ref, watch } from 'vue';

	const props = defineProps(['characterList']);

	let characters = ref([]);

	watch(props.characterList, list => (characters.value = list));

</script>


<template>
	<div class="container">
		<article v-for="char in characters" :key="char.id">
			<div class="div-img">
				<img v-bind:alt="char.name" v-bind:src="char.image">
			</div>
			<div class="div-text-part">
			  <div class="section">
			    <a
			      v-bind:href="char.url"
			      rel="noopener noreferrer"
			      target="_blank"
			    >
						<h2>{{char.name}}</h2>
			    </a>
			    <span class="status">
						<span class="status__icon" :style="
							char.status === 'Alive' 
								? 'background-color: rgb(80, 152, 42);'
								:	char.status === 'unknown'
									? 'background-color: rgb(8, 52, 166);'
									: 'background-color: rgb(255, 12, 12);' 
									"></span><!-- sorry for that shit -->
						{{char.status}} - {{char.species}}
			    </span>
			  </div>
			  <div class="section">
			    <span class="text-gray">Last known location:</span>
			    <a
			      v-bind:href="char.location.url"
			      rel="noopener noreferrer"
			      target="_blank"
			    >
						{{char.location.name}}
			    </a>
			  </div>
			  <div class="section">
			    <span class="text-gray">First seen in:</span>
			    <a
			      v-bind:href="char.episode[0].url"
			      rel="noopener noreferrer"
			      target="_blank"
			    >
						{{char.episode[0].name}}
			    </a>
			  </div>
			</div>
		</article>
	</div>
</template>


<style  scoped>
body{
	word-wrap: break-word;
}
.container{
	display: flex;
  -moz-box-pack: center;
  justify-content: center;
  -moz-box-align: center;
  align-items: center;
  flex-wrap: wrap;
  max-width: 1920px;
}

article{
	background-color: rgb(48, 52, 54);
  background-image: none;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
  width: 600px;
  height: 220px;
  display: flex;
  overflow: hidden;
  background: rgb(60, 62, 68);
    background-color: rgb(60, 62, 68);
    background-image: none;
  border-radius: 0.5rem;
  margin: 0.75rem;
}
.div-img{
	flex: 2 1 0%;
}
img{
	width: 229px;
  height: auto;
  display: block;
  max-width: 100%;
	margin: 0;
	padding: 0;
}

.div-text-part{
	color: rgb(232, 230, 227);
	flex: 3 1 0%;
  position: relative;
  padding: 0.75rem;
  color: rgb(255, 255, 255);
  display: flex;
  flex-direction: column;
}

.div-text-part .section:first-child{
-moz-box-pack: start;
  justify-content: flex-start;
}

.div-text-part .section {
	text-align: left;
  flex: 1 1 0%;
  display: flex;
  flex-direction: column;
  -moz-box-pack: center;
  justify-content: center;
}

a{
	background-color: transparent;
	text-decoration: none;
	color: rgb(226, 223, 219);
}

h2{
  font-size: 1.5rem;
	margin: 0px;
  padding: 0px;
	color: inherit;
	font-family: -apple-system,'BlinkMacSystemFont','Segoe UI','Roboto','Helvetica','Arial',sans-serif,'Apple Color Emoji','Segoe UI Emoji','Segoe UI Symbol';
  font-weight: 800;
  font-size: 1.51572rem;
  line-height: 1.1;
}

h1, h2 {
	border-bottom-color: currentcolor;
	border-bottom: none;
}

.status{
	display: flex;
  -moz-box-align: center;
  align-items: center;
  text-transform: capitalize;
	font-size: 16px;
  font-weight: 500;
	margin: 0px;
  padding: 0px;
}

.status__icon{
  background-image: none;
	height: 0.5rem;
  width: 0.5rem;
  margin-right: 0.375rem;
	border-radius: 50%;
}

.text-gray{
	color: rgb(171, 163, 152);
}
</style>
