<template>
    <div class="home">
      <div class="search-container">
        <input v-model="searchQuery" placeholder="Search movies" class="search-input" />
        <button @click="onSearchButtonClick" class="search-button">Search</button>
      </div>
      <div class="movies-list">
        <router-link v-for="movie in movies" :key="movie.imdbID" :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="movie-card">
            <img :src="movie.Poster" alt="Movie Poster" class="movie-poster" />
            <div class="movie-details">
              <p class="movie-label">{{ movie.Type }}</p>
              <p class="movie-year">{{ movie.Year }}</p>
              <h3 class="movie-title">{{ movie.Title }}</h3>
            </div>
          </div>
        </router-link>
      </div>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import { useRoute } from 'vue-router';
  
  export default {
    name: 'Home',
    data() {
      return {
        searchQuery: '',
        movies: [],
      };
    },
    methods: {
      onSearchButtonClick() {
        if (this.searchQuery) {
         
          const apiKey = '2f5090f2';
          fetch(`http://www.omdbapi.com/?apikey=${apiKey}&s=${this.searchQuery}`)
            .then(response => response.json())
            .then(data => {
              if (data.Search) {
                this.movies = data.Search;
              }
            });
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .home {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
  }
  
  .search-container {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  
  .search-input {
    padding: 10px;
    margin-right: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  
  .search-button {
    padding: 10px 15px;
    background-color: #e50914;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .search-button:hover {
    background-color: #b2070a;
  }
  
  .movies-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 20px;
    justify-items: center;
    margin-top: 20px;
  }
  
  .movie-link {
    text-decoration: none;
    color: inherit;
  }
  
  .movie-card {
    background-color: #000;
    color: #fff;
    border-radius: 5px;
    overflow: hidden;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s;
    position: relative;
  }
  
  .movie-card:hover {
    transform: translateY(-5px); /*use this also */
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
  }
  
  .poster-container {
    position: relative;
    overflow: hidden;
  }
  
  .movie-poster {
    width: 100%;
    height: auto;
    transition: transform 0.3s, filter 0.3s; /*and here finally*/
  }
  
  .movie-card:hover .movie-poster {
    transform: scale(1.05);
    /*! use this filter effect */
    filter: brightness(0.8);
  }
  
  .movie-details {
    padding: 15px;
    text-align: center;
  }
  
  .movie-label {
    font-size: 14px;
    margin-bottom: 5px;
  }
  
  .movie-year {
    font-size: 16px;
    margin: 0;
  }
  
  .movie-title {
    font-size: 18px;
    margin: 10px 0;
  }
  </style>