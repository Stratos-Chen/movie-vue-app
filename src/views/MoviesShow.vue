<template>
  <div class="movie-show">
    <h2>{{ movie.title }}</h2>
    <p>Year: {{ movie.year }}</p>
    <p>Plot: {{ movie.plot }}</p>

    <router-link :to="`/movies/${movie.id}/edit`">Edit</router-link> <br>
    <button v-on:click="destroymovie()">Delete</button>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";

export default {
  data: function() {
    return {
      movie: {}
    };
  },
  created: function() {
    axios.get(`/api/movies/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.movie = response.data;
    });
  },
  methods: {
    destroymovie: function() {
      if (confirm("Are you sure you want to delete this movie?")) {
        axios.delete(`/api/movie/${this.movie.id}`).then(response => {
          console.log("Successfully destroyed", response.data);
          this.$router.push("/movies");
        });
      }
    }
  }
};
</script>