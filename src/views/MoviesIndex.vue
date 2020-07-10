<template>

  <div class="movies-index">
    <label> Search by name: </label>
    <input v-model="titleFilter" lists="titles">
    <datalist id="titles">
      <option v-for="movie in movies">{{ movie.title }}</option>
    </datalist>
    <div v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), 'title')">
      <button>Sort Alphabetically</button>
    </div>
    <div v-for="movie in movies">
      <h2>{{ movie.title }}<h2>
      <p>Year: {{ movie.year }}</p>
        <router-link v-bind:to="`/movies/${movie.id}`">More Info</router-link>
      <button v-on:click="showMovie(movie)">More Info</button>
    </div>

  <dialog id="movie-details">
      <form method="dialog">
        <h2>Title: {{ movie.title }}</h2>
        <p>Body: {{ movie.body }}</p>
        <p>Image: {{ movie.image }} </p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      movies: [],
      currentMovie: {},
      title: "",
      movie: {},
      titleFilter: ""
    };
  },
  created: function() {
    axios.get("/api/movies").then(response => {
      console.log("All Movies:", response.data);
      this.movies = response.data;
    });
  },
  methods: {
    showMovie: function(movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    }
  }
};
</script>