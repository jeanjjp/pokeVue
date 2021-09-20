<template>
	<div class="hello">

		<div class="cabecalho">
			<div class="container">
				<img alt="Vue logo" src="https://upload.wikimedia.org/wikipedia/commons/9/98/International_Pok%C3%A9mon_logo.svg">
				<div class="row">
					<div class="col s10">
						<input placeholder="Dale aqui" @keyup.enter="buscarPokemon" type="text" v-model="valorDeBusca" />
					</div>
					<div class="col s2">
						<a class="btn red" v-on:click="buscarPokemon">Pléu</a>
					</div>
				</div>
			</div>
		</div>

		<transition name="modal-fade">
			<div class="modal-backdrop" v-if="modal">

				<div class="modal">
					<header class="modal-header">
						<h5> <b> {{pokemon.id}} - {{pokemon.name}}</b></h5>
						<a class="btn-small red" v-on:click="fecharModal"><i class="material-icons">close</i></a>
					</header>

					<section class="modal-body">
						<div class="row">

							<div class="img col s12 m6 red">
								<img :src="imgUrl + pokemon.id + '.png'" alt="pokemon.name">
							</div>

							<div class="detalhes col s12 m6">

								<div class="row propriedades">
									<div class="col s6 left"><b>Experiencia Base:</b></div>
									<div class="col s6 right">{{pokemon.base_experience}} XP</div>
									<div class="col s6 left"><b>Altura:</b></div>
									<div class="col s6 right">{{pokemon.height / 10}}m.</div>
									<div class="col s6 left"><b>Peso:</b></div>
									<div class="col s6 right">{{pokemon.weight / 10}}Kg.</div>
								</div>


								<div class="row tipos">
									<h6><b>Pokemon Tipo</b></h6>
									<hr />
									<div class="tipo col" v-for="(valor, i) in pokemon.types" :key="'valor'+i">
										<span :class="valor.type.name">
											{{ valor.type.name }}
										</span>
									</div>
								</div>

								<div class="row habilidades">
									<h6><b>Pokemon Habilidades</b></h6>
									<hr />
									<div class="tipo col" v-for="(valor, i) in pokemon.abilities" :key="'valor'+i">
										<span :class="valor.ability.name">
											{{ valor.ability.name }}
										</span>
									</div>
								</div>

							</div>

						</div>
					</section>

				</div>
			</div>
		</transition>

	</div>

</template>

<script>
	export default {

		data: () => {
			return {
				valorDeBusca: '',
				url: 'https://pokeapi.co/api/v2/pokemon/',
				pokemon: '',
				modal: false,
				imgUrl: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/"
			}
		},
		methods: {
			buscarPokemon() {
				let req = new Request(this.url + this.valorDeBusca);
				fetch(req)
					.then((resp) => {
						if (resp.status === 200) {
							return resp.json();
						}
					})
					.then((data) => {
						if (data.id > 0) {
							this.pokemon = data;
							this.modal = true;
						} else {
							this.pokemon = '';
							this.modal = false;
						}
					})
					.catch((error) => {
						console.log(error);
						this.modal = false;
						this.pokemon = '';
					})
				console.log(this.pokemon);
			},
			fecharModal() {
				this.modal = false;
			}
		}
	}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.modal-backdrop {
		position: fixed;
		top: 0;
		bottom: 0;
		left: 0;
		right: 0;
		background-color: rgba(0, 0, 0, 0.3);
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.modal {
		background: #FFFFFF;
		box-shadow: 2px 2px 20px 1px;
		overflow-x: auto;
		display: flex;
		flex-direction: column;
		border-radius: 20px;
	}

	.modal-header,
	.modal-footer {
		padding: 10px;
		display: flex;
	}

	.modal-header {
		position: relative;
		border-bottom: 1px solid #eeeeee;
		justify-content: space-between;
	}

	.modal-footer {
		border-top: 1px solid #eeeeee;
		flex-direction: column;
	}

	.modal-body {
		position: relative;
		padding: 20px 10px;
	}

	.modal-fade-enter,
	.modal-fade-leave-to {
		opacity: 0;
	}

	.modal-fade-enter-active,
	.modal-fade-leave-active {
		transition: opacity .5s ease;
	}

	.img {
		border-radius: 30px;
	}

	img {
		width: 250px;
		height: 250px;
	}

	i {
		margin-top: 4px;
	}

	.tipo {
		border: solid 1px;
		border-radius: 30px;
		margin-right: 5px;
		margin-left: 5px;
		margin-bottom: 5px;
	}

	hr {
		margin-left: 10px;
	}

	.cabecalho {
		background-color: #FFF;
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
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
