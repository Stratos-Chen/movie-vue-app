<template>
  <div class="home">
    <h1>List of movies</h1>
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>

    <div>
      Title: <input type="text" v-model="newMovieTitle"> <br>
      Year: <input type="text" v-model="newMovieYear"> <br>
      Plot: <input type="text" v-model="newMoviePlot"> <br>
      Director: <input type="text" v-model="newMovieDirector"> <br>
      English: <input type="text" v-model="newMovieEnglish"> <br>
      <button v-on:click="createMovie()">Create new movie</button>
    </div>

    <div v-for="movie in movies">
      <h2>Title: {{ movie.title }}</h2>
      <p>Year: {{ movie.year }}</p>
      <p>Plot: {{ movie.plot }}</p>
      <p>Director: {{ movie.director }}</p>
      <p>English: {{ movie.english }}</p>
      <button v-on:click="showMovie(movie)">More Info</button>
    </div>

    <dialog id="movie-details">
      <form method="dialog">
        <h1>Movie Info</h1>
        <p>Title: <input type="text" v-model="currentMovie.title"></p>
        <p>Year: <input type="text" v-model="currentMovie.year"></p>
        <p>Plot: <input type="text" v-model="currentMovie.plot"></p>
        <p>Director: <input type="text" v-model="currentMovie.director"></p>
        <p>English: <input type="text" v-model="currentMOvie.english"></p>
        <button v-on:click="updateMovie(currentMovie)">Update movie</button>
        <button v-on:click="destroyMovie(currentMovie)">Delete movie</button>
        <button>Close</button>
      </form>
    </dialog>


  </div>
</template>

<style>
</style>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  data: function() {
    return {
      movies: [],
      errors: [],
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
      newMovieDirector: "",
      newMovieEnglish: "",
      currentMovie: {}
    };
  },
  created: function() {
    this.indexMovies();
  },
  methods: {
    indexMovies: function() {
      axios.get("/api/movies").then(response => {
        console.log("All Movies:", response.data);
        this.movies = response.data;
      });
    },
    createMovie: function() {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
        director: this.newMovieDirector,
        english: this.newMovieEnglish
      };
      axios
        .post("/api/movies", params)
        .then(response => {
          console.log("Success", response.data);
          this.movies.push(response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
    showMovie: function(movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    },
    updateMovie: function(movie) {
      var params = {
        title: movie.title,
        year: movie.year,
        plot: movie.plot,
        director: movie.director,
        english: movie.english
      };
      axios
        .patch(`/api/movies/${movie.id}`, params)
        .then(response => {
          console.log("Successful update", response.data);
        })
        .catch(error => {
          console.log(error.response.data.erros);
        });
    },
    destroyMovie: function(movie) {
      axios.delete(`/api/movies/${movie.id}`).then(response => {
        console.log("Successful delete", response.data);
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    }
  }
};
</script>
