<template>
  <div class="hikes-index">
    <h1>All Hikes</h1>
    <div v-for="hike in filterBy{hikes, difficultyFilter, 'difficulty_level'}">
      <h2>
        <router-link :to="`/hikes/${hike.id}`">{{ hike.name }}</router-link>
      </h2>
      <p>Description: {{ hike.description }}</p>
      <h3 class="difficulty_level">Difficulty: {{ hike.difficulty_level }}</h3>
    </div>
    <br />
    <br />
    <router-link to="/hikes/new">Create A Hike</router-link>
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
