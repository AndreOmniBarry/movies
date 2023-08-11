<!--Home.vue-->
<template>
  <h1>CinePedia</h1>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0099785">
        <img src="https://m.media-amazon.com/images/M/MV5BMzFkM2YwOTQtYzk2Mi00N2VlLWE3NTItN2YwNDg1YmY0ZDNmXkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg" alt="Home Alone Poster" class="featured-img" />
        <div class="detail">
          <h3>Home Alone</h3>
          <p>An eight-year-old troublemaker, mistakenly left home alone, must defend his home</p>
        </div>
      </router-link>
    </div>


   <form @submit.prevent="SearchMovies()" class="search-box">
   <input type="text" placeholder="Search Movies..." v-model="search" />
   <input type="submit" value="Search" />
   </form>

   <div class="movies-list">
    <div class="movie" v-for="movie in movies" :key="movie.imdbID">
    <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
    <div class="product-image">
      <img src="movie.Poster" alt="Movie Poster" />
      <div class="type"> {{ movie.Type }}</div>
    </div>
    <div class="detail">
      <p class="year">{{ movie.Year }}</p>
      <h3> {{ movie.Title }}</h3>
    </div>
    </router-link>
    </div>
   </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

const API_KEY = 'ad6d4442';
const searchQuery = ref('');
const results = reactive([]);
const isLoading = ref(false);

const search = () => {
  if (searchQuery.value.trim() === '') {
    results.length = 0;
    return;
  }

  isLoading.value = true;

  axios.get('http://www.omdbapi.com/', {
    params: {
      apikey: API_KEY,
      i: 'tt3896198',
      s: searchQuery.value,
    },
  })
    .then(response => {
      results.splice(0, results.length, ...(response.data.Search || []));
      isLoading.value = false;
    })
    .catch(error => {
      console.error('Error fetching data:', error);
      isLoading.value = false;
    });
};

// Function to navigate to the movie details page
const navigateToMovieDetails = (imdbID) => {
  // Redirect to the movie details page with the IMDb ID as a parameter
  router.push({ name: 'movie-details', params: { id: imdbID } });
};
</script>

<style>
.home .feature-card {
  position: relative;
}

.home .feature-card .featured-img {
  display: block;
  width: 100%;
  height: 300px;
  object-fit: cover;
  position: relative;
  z-index: 0;
}

.home .feature-card .detail {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.6);
  padding: 16px;
  z-index: 1;
}

.home .feature-card .detail h3 {
  color: #FFF;
  margin-bottom: 16px;
}

.home .feature-card .detail p {
  color: #FFF;
}

.home .search-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 16px;
}

.home .search-box input {
  display: block;
  appearance: none;
  border: none;
  outline: none;
  background: none;
}

.home .search-box input[type="text"] {
  width: 100%;
  color: #FFF;
  background-color: #496583;
  font-size: 20px;
  padding: 10px 16px;
  border-radius: 8px;
  margin-bottom: 15px;
  transition: 0.4s;
}

.home .search-box input[type="text"]::placeholder {
  color: #f3f3f3;
}

.home .search-box input[type="text"]:focus {
  box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
}

.home .search-box input[type="submit"] {
  width: 100%;
  max-width: 300px;
  background-color: #42B883;
  padding: 16px;
  border-radius: 8px;
  color: #FFF;
  font-size: 20px;
  text-transform: uppercase;
  transition: 0.4s;
}

.home .search-box input[type="submit"]:active {
  background-color: #3B8070;
}

.home .movies-list {
  display: flex;
  flex-wrap: wrap;
  margin: 0px 8px;
}

.home .movies-list .movie {
  max-width: 50%;
  flex: 1 1 50%;
  padding: 16px 8px;
}

.home .movies-list .movie .movie-link {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.home .movies-list .movie .movie-link .product-image {
  position: relative;
  display: block;
}

.home .movies-list .movie .movie-link .product-image img {
  display: block;
  width: 100%;
  height: 275px;
  object-fit: cover;
}

.home .movies-list .movie .movie-link .product-image .type {
  position: absolute;
  padding: 8px 16px;
  background-color: #42B883;
  color: #FFF;
  bottom: 16px;
  left: 0px;
  text-transform: capitalize;
}

.home .movies-list .movie .movie-link .detail {
  background-color: #496583;
  padding: 16px 8px;
  flex: 1 1 100%;
  border-radius: 0px 0px 8px 8px;
}

.home .movies-list .movie .movie-link .detail .year {
  color: #AAA;
  font-size: 14px;
}

.home .movies-list .movie .movie-link .detail h3 {
  color: #FFF;
  font-weight: 600;
  font-size: 18px;
}
</style>
