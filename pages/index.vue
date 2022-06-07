<template>
  <div class="home">
    <!-- Hero -->
    <HeroComponent />

    <!-- Movies -->
    <div class="container movies">
      <div id="movie-grid" class="movies-grid">
        <div v-for="(movie, index) in movies" :key="index" class="movie">
          <div class="movie-img">
            <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="">
            <p class="review">{{movie.vote_average}}</p>
            <p class="overview">{{movie.overview}}</p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25) }} 
              <span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric'
                })
              }}
            </p>
            <NuxtLink class="button button-light" :to="{name: 'movies-movieid', params: {movieid: movie.id}}">
              Get More Info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import HeroComponent from '../components/HeroComponent.vue';
require('dotenv').config()
export default {
    name: "IndexPage",
    components: { HeroComponent },
    data() {
      return {
        movies: [],
      }
    },
    async fetch() {
      await this.getMovies();
    },
    methods: {
      async getMovies() {
        const data = axios.get('URL');
        const response = await data;
        const results = response.data.results;
        results.forEach((movie) => {
          this.movies = [...this.movies, movie]
        });
      }
    }
}
</script>

<style lang="scss" scoped>
.movies {
  padding: 32px 16px;
  column-gap: 32px;
  row-gap: 64px;
  @media screen {
    
  }
}
</style>