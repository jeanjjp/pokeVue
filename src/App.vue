<template>
	<div id="app">
		<PokeModal />
		<PokeLista msg="" />
	</div>
</template>

<script>
	import PokeModal from './components/PokeModal.vue'
	import PokeLista from './components/PokeLista.vue'

	export default {
		name: 'App',
		components: {
			PokeModal,
			PokeLista
		}
	}

</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}

</style>
