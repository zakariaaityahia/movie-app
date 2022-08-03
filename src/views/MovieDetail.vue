<template>
  <div class="movie-detail">
    <h2>{{movie.Title}}</h2>
    <p> Year :{{movie.Year}}</p>
    <p>Genre: {{movie.Genre}}</p>
    <p>Rating: {{movie.Rated}}</p>
    <p>Releasing Date: {{movie.Released}}</p>
    <p>Imdb Votes: {{movie.imdbVotes}}</p>
    <img :src="movie.Poster" alt="Movie Poster" class="featured-img">
    <p>{{movie.Plot}}</p>
  </div>
</template>

<script>
import { ref, onBeforeMount } from 'vue';
import { useRoute } from 'vue-router';
import env from '@/env.js';

export default {
  setup() {
    const movie = ref({});
    const route = useRoute();

    onBeforeMount(() => {
      fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`)
        .then(res => res.json())
        .then(data => {
          console.log(data)
          movie.value = data;
      })
    });
    return {
      movie 
    }
  }
}
</script>

<style lang="scss">
.movie-detail {
  padding: 16px;

  h2 {
    color: #000000;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
  }

  .featured-img {
    display: block;
    max-width: 200px;
    margin-bottom: 16px;
  }
// s
  p {
    color: #000000;
    font-size: 18px;
    line-height: 1.4;
  }
}
</style>