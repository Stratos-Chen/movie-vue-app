<template>
  <div class="home">
    <form v-on:submit.prevent="submit()">
      <h1>Create New Movies</h1>

      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="newMovieTitle">
      </div>
      <div>
        <label>Year:</label>
        <input type="text" class="form-control" v-model="newMovieYear">
      </div>
      <div>
        <label>Plot:</label>
        <input type="text" class="form-control" v-model="newMoviePlot">
      </div>
      <input type="submit" class="btn btn-primary" value="Submit">
    </form>
  </div>
</template>

<script>
import axios from "axios";
// @ is an alias to /src

export default {
  data: function() {
    return {
      movies: [],
      errors: [],
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: ""
    };
  },
  methods: {
    createMovie: function() {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot
      };
      axios
        .movie("/api/movies", params)
        .then(response => {
          console.log("Success", response.data);
          this.movies.push(response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>