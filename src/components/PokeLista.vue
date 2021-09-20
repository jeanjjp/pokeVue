<template>
	<div class="hello">
		<h1>{{ msg }}</h1>

		<div class="row">
			<div class="container">
				<div class="lista">
					<div class="col m3 s6" v-for="(pokemon, index) in pokemons" :key="'poke'+index">
						<div class="pokes red z-depth-5">
							<img :src="imgUrl + pokemon.id + '.png'" width="96" height="96" alt="">
							<h5>{{ pokemon.name }}</h5>
						</div>
					</div>
				</div>
			</div>
		</div>

		<a class="btn-large red" v-on:click="mais"><i class="material-icons">expand_more</i></a>
	</div>

</template>

<script>
	export default {
		props: [
			'msg'
		],
		data: () => {
			return {
				url: 'https://pokeapi.co/api/v2/pokemon/',
				pokemon: '',
				pokemons: [],
				nextUrl: '',
				imgUrl: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/"
			}
		},
		methods: {
			dale() {
				let req = new Request(this.url);
				fetch(req)
					.then((resp) => {
						if (resp.status === 200)
							return resp.json();
					})
					.then((data) => {
						this.nextUrl = data.next;
						data.results.forEach(pokemon => {
							pokemon.id = pokemon.url.split('/')
								.filter(function(part) {
									return !!part
								}).pop();
							this.pokemons.push(pokemon);
						});
					})
					.catch((error) => {
						console.log(error);
					})
			},
			mais() {
				let req = new Request(this.nextUrl);
				fetch(req)
					.then((resp) => {
						if (resp.status === 200)
							return resp.json();
					})
					.then((data) => {
						this.nextUrl = data.next;
						data.results.forEach(pokemon => {
							pokemon.id = pokemon.url.split('/')
								.filter(function(part) {
									return !!part
								}).pop();
							this.pokemons.push(pokemon);
						});
					})
					.catch((error) => {
						console.log(error);
					})
			}

		},
		created() {
			this.url = 'https://pokeapi.co/api/v2/pokemon/';
			this.dale();
			console.log("foi");
		}

	}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.lista {
		margin-top: 350px;
	}

	.pokes{
		padding-top: 10px;
		color: #fff;
		border-radius: 10px;
		margin-bottom: 15px;
	}
	/* fallback */
	@font-face {
		font-family: 'Material Icons';
		font-style: normal;
		font-weight: 400;
		src: url(https://fonts.gstatic.com/s/materialicons/v103/flUhRq6tzZclQEJ-Vdg-IuiaDsNcIhQ8tQ.woff2) format('woff2');
	}

	.material-icons {
		font-family: 'Material Icons';
		font-weight: normal;
		font-style: normal;
		font-size: 24px;
		line-height: 1;
		letter-spacing: normal;
		text-transform: none;
		display: inline-block;
		white-space: nowrap;
		word-wrap: normal;
		direction: ltr;
		-webkit-font-feature-settings: 'liga';
		-webkit-font-smoothing: antialiased;
	}

</style>
