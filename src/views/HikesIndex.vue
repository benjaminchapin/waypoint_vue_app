<template>
  <div class="hikes-index">
    <h1>All Hikes</h1>
    <div>
      <label for="difficulty_levels">Sort By Difficulty Level:</label>
      <br />
      <select id="difficulty_levels" type="text" class="form-control" v-model="difficultyFilter">
        <option value="novice">Novice</option>
        <option value="intermediate">Intermediate</option>
        <option value="advanced">Advanced</option>
      </select>
    </div>
    <div v-for="hike in filterBy(hikes, difficultyFilter, 'difficulty_level')">
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
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      hikes: [],
      difficultyFilter: ""
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
