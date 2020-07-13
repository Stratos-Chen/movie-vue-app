<template>
  <div id="app">
    <header id="header" class="text-center tm-text-gray">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
      <span v-if="isLoggedIn()">
        <router-link to="/signup">Signup</router-link> 
      </span>
      <span v-else>
        <router-link to="/login">Login</router-link> |
        <router-link to="/logout">Logout</router-link>
      </span>
      <h1>CENTRAL</h1>
      <p>NEW TEMPLATE FOR YOU</p>
    </header>
    <router-view/>

    <footer class="mt-5">
      <p class="text-center">Copyright © 2018 Your Company Name - Design: Tooplate</p>
    </footer>
  </div>
</template>

<style>
</style>

<script>
export default {
  methods: {
    isLoggedIn: function() {
      return localStorage.getItem("jwt");
    }
  }
};
</script>
