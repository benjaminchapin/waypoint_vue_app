<template>
  <div class="hikes-index">
    <h1>All Hikes</h1>
    <div v-for="hike in hikes">
      <h2>{{ hike.name }}</h2>
      <p>Description: {{ hike.description }}</p>
      <p>Difficulty: {{ hike.difficulty_level }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      hikes: []
    };
  },
  created: function() {
    axios.get("/api/hikes").then(response => {
      this.hikes = response.data;
    });
  },
  methods: {}
};
</script>
