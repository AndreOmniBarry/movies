<template>
    <div class="movie-detail">
      <div class="movie-header">
        <h2 class="movie-title">{{ movie.Title }}</h2>
        <p class="movie-year">{{ movie.Year }}</p>
      </div>
      <div class="movie-poster">
        <img :src="movie.Poster" alt="Movie Poster" />
      </div>
      <div class="movie-details">
        <p class="detail-label">Viewer Ratings: {{ movie.imdbRating }}/10</p>
        <p class="detail-label">Director: {{ movie.Director }}</p>
        <p class="detail-label">Producer: {{ movie.Production }}</p>
        <p class="detail-label">Cast: {{ movie.Actors }}</p>
        <p class="detail-label">Release Date: {{ movie.Released }}</p>
        <p class="detail-label">Critic Ratings: {{ movie.Metascore }}/100</p>
        <p class="detail-label">Budget: {{ movie.BoxOffice }}</p>
        <h3 class="plot-title">Plot</h3>
        <p class="movie-plot">{{ movie.Plot }}</p>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onBeforeMount } from 'vue';
  import { useRoute } from 'vue-router';
  
  export default {
    name: 'MovieDetail',
    setup() {
      const movie = ref({});
      const route = useRoute();
  
      onBeforeMount(() => {
        const apiKey = '2f5090f2'; // Replace with your actual API key
        fetch(`http://www.omdbapi.com/?apikey=${apiKey}&i=${route.params.id}&plot=full`)
          .then(response => response.json())
          .then(data => {
            movie.value = data;
          });
      });
  
      return { movie };
    },
  };
  </script>
  
  <style scoped>
  .movie-detail {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
  }
  
  .movie-header {
    text-align: center;
    margin-bottom: 20px;
  }
  
  .movie-title {
    font-size: 24px;
    margin: 0;
  }
  
  .movie-year {
    font-size: 16px;
    color: #555;
  }
  
  .movie-poster img {
    max-width: 100%;
    height: auto;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .movie-details {
    margin-top: 20px;
    max-width: 600px;
    text-align: justify;
  }
  
  .detail-label {
    margin: 5px 0;
    font-size: 14px;
  }
  
  .plot-title {
    font-size: 18px;
    margin-top: 20px;
  }
  
  .movie-plot {
    margin-top: 10px;
  }
  
  /* ... (other styles) */
  </style>
  