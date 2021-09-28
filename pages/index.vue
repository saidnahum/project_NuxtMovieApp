<template>
	<div class="home">
		<!-- Hero -->
      <Hero />

		<!-- Movies -->
		<div class="container mx-auto movies px-8 py-4">
			<div id="movie-grid grid" class="my-10 grid sm:grid-cols-2 md:grid-cols-3 xl:grid-cols-4 gap-8">
				<div class="movie relative flex flex-col" v-for="(movie, index) in movies" :key="index">
					<div class="movie-img relative overflow-hidden">
						<img 
							:src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" 
							alt=""
						/>
						<p class="review absolute top-0 left-0 flex justify-center items-center w-10 h-10 bg-red-600 text-white rounded-br-2xl">{{movie.vote_average}}</p>
						<p class="overview absolute bottom-0 bg-red-600 p-3 text-white text-sm">{{movie.overview}}</p>
					</div>
					<div class="info mt-auto">
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
		</div>
   </div>
</template>

<script>
import axios from 'axios';
export default {
	data() {
		return {
			movies: []
		}
	},
	async fetch(){
		await this.getMovies();
	},
	methods: {
		async getMovies(){
			const data = axios.get(
				'https://api.themoviedb.org/3/movie/now_playing?api_key=52366e550def9f544fe0d4ce80659837&language=es-ES&page=1'
			)

			const result = await data;
			result.data.results.forEach((movie) => {
				this.movies.push(movie)
			})
			console.log(this.movies);
		}
	}
}
</script>

<style lang="scss" scoped>
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