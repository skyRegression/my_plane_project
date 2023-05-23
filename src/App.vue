<template>
	<div id="app">
		<AppIndex />
	</div>
</template>

<script>
	import AppIndex from './components/AppIndex.vue'

	export default {
		name: 'App',
		components: {
			AppIndex
		}
	}
</script>

<style>
	html,
	body {
		background-color: lightblue;
		margin: 0;
		height: 100vh;
	}

	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
	}
</style>

