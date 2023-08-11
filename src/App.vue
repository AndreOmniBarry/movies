<!--App.vue--><template>
  <header>
    <router-link to="/">
      <h1><span>Vue</span>Movies</h1>
    </router-link>
  </header>
  <main>
    <router-view />
  </main>
  </template>

 <script setup>
  import { ref, reactive } from 'vue';
  import axios from 'axios';
  import router from './router';

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
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;

  &::selection {
    background: transparent(#42B883, 0.5);
    }
}

body{
  background-color: #d74910;
}


a{
  text-decoration: none;
}

header {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px 16px;
  background-color: #2C3D;
  box-shadow: 0px, 0px, 6px rgba(0, 0, 0, 0.1);

h1 {
  color: #fff;
  font-size: 28px;

  span {
    color: #42B883;
  }
}
}
</style>