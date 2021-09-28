<template>
   <div class="hero flex justify-left items-center bg-cover bg-center sm:bg-left pl-6" style="">
      <div>
         <h3 class="text-red-600 text-2xl sm:text-4xl uppercase font-bold">Nuevas Películas</h3>
         <h1 class="text-white my-2 sm:my-0 text-5xl sm:text-7xl mb-5"><span class="font-bold">Ahora</span> en Streaming</h1>
         <a href="#movie-grid" class="bg-red-600 text-white py-2 px-3 inline-block rounded mt-5 font-bold text-md">Ver Películas</a>
      </div>
   </div>
</template>

<script>
export default {
   name: 'Hero',
}
</script>

<style lang="scss" scoped>
   .hero {
      height: 455px;
      background-image: linear-gradient(rgb(0 0 0 / 60%), rgb(0 0 0 / 60%)), url('../assets/imgs/movieHero.jpg');
   }
</style>