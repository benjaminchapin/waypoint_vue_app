<template>
  <div id="app">
    <!-- =========== Header =========== -->
    <header class="clearfix">
      <div class="logo">
        <a href="index.html"><img src="/images/design/WaypointLogo.png" alt="TWILLI Sky" id="logo_dark" /></a>
      </div>
      <!-- .logo -->

      <div id="main-menu-container" class="clearfix">
        <div id="mobile-menu-icon" class="btn btn-outline-inverse visible-xs">
          <span class="glyphicon glyphicon-th"></span>
        </div>

        <ul id="main-menu">
          <router-link v-if="isLoggedIn()" to="/hikes" v-slot="{ href, route, navigate, isActive, isExactActive }">
            <li class="menu-item" :class="[isActive && 'router-link-active', isExactActive && 'current-menu-item']">
              <a :href="href" @click="navigate">Hikes</a>
            </li>
          </router-link>
          <router-link v-if="isLoggedIn()" to="/hikes/new" v-slot="{ href, route, navigate, isActive, isExactActive }">
            <li class="menu-item" :class="[isActive && 'router-link-active', isExactActive && 'current-menu-item']">
              <a :href="href" @click="navigate">New Hike</a>
            </li>
          </router-link>
          <router-link
            v-if="isLoggedIn()"
            :to="`/users/${getUserId()}`"
            v-slot="{ href, route, navigate, isActive, isExactActive }"
          >
            <li class="menu-item" :class="[isActive && 'router-link-active', isExactActive && 'current-menu-item']">
              <a :href="href" @click="navigate">My Account</a>
            </li>
          </router-link>
          <router-link v-if="isLoggedIn()" to="/logout" v-slot="{ href, route, navigate, isActive, isExactActive }">
            <li class="menu-item" :class="[isActive && 'router-link-active', isExactActive && 'current-menu-item']">
              <a :href="href" @click="navigate">Logout</a>
            </li>
          </router-link>
          <router-link v-if="!isLoggedIn()" to="/login" v-slot="{ href, route, navigate, isActive, isExactActive }">
            <li class="menu-item" :class="[isActive && 'router-link-active', isExactActive && 'current-menu-item']">
              <a :href="href" @click="navigate">Login</a>
            </li>
          </router-link>
          <router-link v-if="!isLoggedIn()" to="/signup" v-slot="{ href, route, navigate, isActive, isExactActive }">
            <li class="menu-item" :class="[isActive && 'router-link-active', isExactActive && 'current-menu-item']">
              <a :href="href" @click="navigate">Signup</a>
            </li>
          </router-link>
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

<!-- <style>
#main-menu-container a.router-link-exact-active {
  color: #42b983;
}
</style> -->

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
