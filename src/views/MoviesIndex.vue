<template>
  <div class="movies-index">
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

export default {
  data: function() {
    return {
      movies: [],
      currentMovie: {},
      title: "",
      movie: {}
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