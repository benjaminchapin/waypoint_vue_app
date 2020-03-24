<template>
  <div id="app">
    <!-- <div id="nav">
      <li class="nav-item">
        <router-link class="nav-link" to="/">Home</router-link>
      </li>
      <li v-if="isLoggedIn()" class="nav-item">
        <router-link class="nav-link" :to="`/users/${getUserId()}`">Your Account</router-link>
      </li>
      <li class="nav-item">
        <router-link v-if="isLoggedIn()" to="/hikes">Hikes Index</router-link>
      <li class="nav-item">
        <router-link v-if="isLoggedIn()" to="/logout">Logout</router-link>
      <li class="nav-item">
        <router-link v-if="!isLoggedIn()" to="/login">Login</router-link>
      <li class="nav-item">
        <router-link v-if="!isLoggedIn()" to="/signup">Signup</router-link>
      </li>
    </div> -->

    <!-- =========== Header =========== -->
    <header class="clearfix">
      <div class="logo">
        <a href="index.html"><img src="/images/design/logo.png" alt="TWILLI Sky" id="logo_dark" /></a>
      </div>
      <!-- .logo -->

      <div id="main-menu-container" class="clearfix">
        <div id="mobile-menu-icon" class="btn btn-outline-inverse visible-xs">
          <span class="glyphicon glyphicon-th"></span>
        </div>

        <ul id="main-menu">
          <li v-if="isLoggedIn()" class="menu-item">
            <router-link to="/hikes">Hikes</router-link>
          </li>
          <li v-if="isLoggedIn()" class="menu-item">
            <router-link to="/hikes/new">Create A Hike</router-link>
          </li>
          <li v-if="isLoggedIn()" class="menu-item">
            <router-link :to="`/users/${getUserId()}`">My Account</router-link>
          </li>
          <li v-if="isLoggedIn()" class="menu-item">
            <router-link to="/logout">Logout</router-link>
          </li>
          <li v-if="!isLoggedIn()" class="menu-item">
            <router-link to="/login">Login</router-link>
          </li>
          <li v-if="!isLoggedIn()" class="menu-item">
            <router-link to="/signup">Signup</router-link>
          </li>
        </ul>
        <!-- #main-menu -->
      </div>
      <!-- #menu-container -->
    </header>
    <!-- End: Header -->

    <!-- =========== Page Body Inside Content =========== -->
    <div class="inside-body-content-container clearfix">
      <router-view />
    </div>
    <!-- .inside-body-content-container -->
    <!-- End: Page Body Inside Content -->
  </div>
</template>

<style>
/*#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}*/
</style>

<script>
export default {
  methods: {
    isLoggedIn: function() {
      if (localStorage.getItem("jwt")) {
        return true;
      } else {
        return false;
      }
    },
    getUserId: function() {
      return localStorage.getItem("user_id");
    }
  }
};
</script>
