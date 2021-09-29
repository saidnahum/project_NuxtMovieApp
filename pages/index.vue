<template>
	<div class="home">
		<!-- Hero -->
      <Hero />

		<!-- Search -->
		<div class="flex sm:justify-center sm:items-center mt-5">
			<input v-model.lazy="searchInput" @keyup.enter="$fetch" type="text" placeholder="Busacr Película" class=" p-2 w-full sm:w-96 outline-none">
			<button v-show="searchInput !== ''" @click="clearSearch" class="text-white bg-red-600 p-2 rounded-r-md"><span class="text-sm">Limpiar Búsqueda</span></button>
		</div>

		<!-- Loading -->
		<Loading v-if="$fetchState.pending"/>

		<!-- Movies -->
		<div class="container mx-auto movies px-8 py-4">
			<!-- Search Results -->
			<div id="movie-grid grid" v-if="searchInput !== ''" class="my-10 grid sm:grid-cols-2 md:grid-cols-3 xl:grid-cols-4 gap-8">
				<div v-for="(movie, index) in searchedMovies" :key="index" class="movie relative flex flex-col">
					<div class="movie-img relative overflow-hidden">
						<img 
							:src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" 
							alt=""
						/>
						<p class="review shadow-lg absolute top-0 left-0 flex justify-center items-center w-10 h-10 bg-red-600 text-white rounded-br-2xl">{{movie.vote_average}}</p>
						<p class="overview absolute bottom-0 bg-red-600 p-3 text-white text-sm">{{movie.overview}}</p>
					</div>
					<div class="info mt-auto text-center">
						<p class="title mt-2.5 text-white text-lg">{{movie.title.slice(0, 25)}} <span v-if="movie.title.length > 25">...</span> </p>
						<p class="release mt-1 mb-4 text-gray-400">
							Released:
							{{
								new Date(movie.release_date).toLocaleString('es-es', {
									month: 'long',
									day: 'numeric',
									year: 'numeric',
								})
							}}
						</p>
						<nuxt-link class="text-white px-2 py-2 rounded border border-red-800 hover:bg-red-800 " :to="{name: 'movies-movieid', params: {movieid: movie.id}}">
							Más Información
						</nuxt-link>
					</div>
				</div>
			</div>

			<!-- Now Streaming  -->
			<div id="movie-grid grid" v-else class="my-10 grid sm:grid-cols-2 md:grid-cols-3 xl:grid-cols-4 gap-8">
				<div v-for="(movie, index) in movies" :key="index" class="movie relative flex flex-col">
					<div class="movie-img relative overflow-hidden">
						<img 
							:src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" 
							alt=""
						/>
						<p class="review shadow-lg absolute top-0 left-0 flex justify-center items-center w-10 h-10 bg-red-600 text-white rounded-br-2xl">{{movie.vote_average}}</p>
						<p class="overview absolute bottom-0 bg-red-600 p-3 text-white text-sm">{{movie.overview}}</p>
					</div>
					<div class="info mt-auto text-center">
						<p class="title mt-2.5 text-white text-lg">{{movie.title.slice(0, 25)}} <span v-if="movie.title.length > 25">...</span> </p>
						<p class="release mt-1 mb-4 text-gray-400">
							Estreno:
							{{
								new Date(movie.release_date).toLocaleString('es-es', {
									month: 'long',
									day: 'numeric',
									year: 'numeric',
								})
							}}
						</p>
						<nuxt-link class="text-white px-2 py-2 rounded border border-red-800 hover:bg-red-800 " :to="{name: 'movies-movieid', params: {movieid: movie.id}}">
							Más Información
						</nuxt-link>
					</div>
				</div>
			</div>
		</div>
   </div>
</template>

<script>
export default {
	name: 'home',
	head(){
		return {
			title: 'Movie App - Ahora en Streaming',
			meta: [
				{
					hid: 'description',
					name: 'description',
					content: 'Obten las últimas películas en cine y streaming'
				},
				{
					hid: 'keywords',
					name: 'keywords',
					content: 'movies, películas, streaming'
				}
			]
		}
	},
	data() {
		return {
			movies: [],
			searchedMovies: [],
			searchInput: '',
		}
	},
	
	async fetch() {
		if (this.searchInput === '') {
			await this.getMovies()
			return
		}
		if (this.searchInput !== '') {
			await this.searchMovies()
      }
	},
	fetchDelay: 1000,
	methods: {
		async getMovies() {
			const data = this.$axios.get(
				`https://api.themoviedb.org/3/movie/now_playing?api_key=52366e550def9f544fe0d4ce80659837&language=es-ES&page=1`
			)
			const result = await data
			this.movies = result.data.results
		},
		async searchMovies() {
			const data = this.$axios.get(
				`https://api.themoviedb.org/3/search/movie?api_key=52366e550def9f544fe0d4ce80659837&language=es-ES&page=1&query=${this.searchInput}`
			)
			const result = await data
			this.searchedMovies = result.data.results
		},
		clearSearch() {
			this.searchInput = ''
			this.searchedMovies = []
		}
	},
}
</script>

<style lang="scss" scoped>
	.loading {
		padding-top: 120px;
		align-items: flex-start;
	}
	.movie-img {
		&:hover {
			.overview {
				transform: translateY(0);
			}
		}

		.overview {
			transform: translateY(100%);
         transition: 0.3s ease-in-out all;
		}
	}
</style>