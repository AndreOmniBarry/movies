<!--MovieDetail.vue-->
<template>
  <div>
    <h1>{{ movie.Title }}</h1>
    <p>{{ movie.Plot }}</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const API_KEY = 'ad6d4442';
const movie = ref(null);

onMounted(async () => {
  const imdbID = $route.params.id;
  const response = await axios.get('http://www.omdbapi.com/', {
    params: {
      apikey: API_KEY,
      i: imdbID,
    },
  });
  movie.value = response.data;
});
</script>

<style>
.movie-detail {
  padding: 16px;
}

.movie-detail h2 {
  color: #FFF;
  font-size: 28px;
  font-weight: 600;
  margin-bottom: 16px;
}

.movie-detail .featured-img {
  display: block;
  max-width: 200px;
  margin-bottom: 16px;
}

.movie-detail p {
  color: #FFF;
  font-size: 18px;
  line-height: 1.4;
}

</style>