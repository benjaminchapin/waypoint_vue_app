<template>
  <div class="container">
    <h2>{{ user.first_name }} {{ user.last_name }}</h2>
    <p>Email: {{ user.email }}</p>
    <p>Skill Level: {{ user.skill_level }}</p>
    <router-link to="/index">Back to Hike Index</router-link>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      photo: {}
    };
  },
  created: function() {
    axios.get("/api/users/" + this.$route.params.id).then(response => {
      this.user = response.data;
    });
  },
  methods: {}
};
</script>
