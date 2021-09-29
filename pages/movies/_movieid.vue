<template>
   <Loading v-if="$fetchState.pending"/>
   <div v-else>
      <div class="container mx-auto">
         <nuxt-link :to="{name: 'index'}">.</nuxt-link>
         <section class="text-blueGray-700 bg-white mt-0 sm:mt-20">
            <div class="container flex flex-col items-center px-5 py-0 sm:py-16 mx-auto md:flex-row lg:px-28">
               <div class="w-full lg:w-5/6 lg:max-w-lg md:w-1/2 p-10">
                  <img class="object-cover object-center rounded-lg" alt="hero" :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`">
               </div>
               <div class="flex flex-col items-start w-full pt-0 mb-16 text-left  lg:flex-grow md:w-1/2 xl:ml-20 md:pr-24 md:mb-0">
                  <h1 class="mb-8 text-lg sm:text-2xl font-black tracking-tighter text-black  md:text-5xl title-font"><span class="text-gray-500">Título:</span> {{ movie.title }}</h1>
                  <h2 class="mb-2 text-base lg:text-xl leading-relaxed text-left"><span class="font-bold">Tagline:</span> "<span class="italic">{{movie.tagline}}</span>"</h2>
                  <h2 class="mb-2 text-base lg:text-xl leading-relaxed text-left"><span class="font-bold">Estreno:</span> {{ new Date(movie.release_date).toLocaleString('es-es', {month: 'long',day: 'numeric', year: 'numeric',}) }}.</h2>
                  <h2 class="mb-2 text-base lg:text-xl leading-relaxed text-left"><span class="font-bold">Duración:</span> {{movie.runtime}} minutos.</h2>
                  <h2 class="mb-2 text-base lg:text-xl leading-relaxed text-left"><span class="font-bold">Ingresos:</span> ${{movie.revenue.toLocaleString('es-es', {style: 'currency', currency: 'MXN'})}}.</h2>
                  <h2 class="mb-8 text-base lg:text-xl leading-relaxed text-left"><span class="font-bold">Resumen:</span> <span class="">{{movie.overview}}</span></h2>
                  <div class="flex flex-col text-center w-auto sm:w-full gap-2 md:justify-start md:flex-row">                        
                     <nuxt-link :to="{name: 'index'}" class="flex items-center px-6 py-3 mt-auto font-semibold text-white transition duration-500 ease-in-out transform bg-red-600 rounded-lg  hover:bg-red-700 focus:shadow-outline focus:outline-none focus:ring-2 ring-offset-current ring-offset-2"> Atras </nuxt-link>
                  </div>
               </div>
            </div>
         </section>
      </div>
   </div>
</template>

<script>
export default {
   name: 'single-movie',
   head(){
      return {
         title: this.movie.title
      }
   },
   data() {
      return {
         movie: ''
      }
   },
   async fetch(){
      await this.getSingleMovie();
   },
   fetchDelay: 1000,
   methods: {
      async getSingleMovie(){
         const data = this.$axios.get(
            `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=52366e550def9f544fe0d4ce80659837&language=es-ES`
         )

         const result = await data;
         this.movie = result.data
         console.log(this.movie);
      }
   }
}
</script>

<style>

</style>