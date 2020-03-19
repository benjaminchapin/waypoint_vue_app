<template>
  <div class="hikes-new">
    <div class="container">
      <form v-on:submit.prevent="createHike()">
        <h1>New Hike</h1>
        <img v-if="status" v-bind:src="`https://http.cat/${status}`" />
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          <label>Name:</label>
          <input type="text" class="form-control" v-model="name" />
        </div>
        <div class="form-group">
          <label>Description:</label>
          <input type="text" class="form-control" v-model="description" />
        </div>

        <div class="form-group">
          <label for="difficulty_levels">Difficulty Level:</label>
          <select id="difficulty_levels" type="text" class="form-control" v-model="difficultyLevel">
            <option value="novice">Novice</option>
            <option value="intermediate">Intermediate</option>
            <option value="advanced">Advanced</option>
          </select>
        </div>

        <div class="form-group">
          <label>Start Address:</label>
          <input type="text" class="form-control" v-model="startAddress" />
        </div>
        <div class="form-group">
          <label>End Address:</label>
          <input type="text" class="form-control" v-model="endAddress" />
        </div>
        <input type="submit" class="btn btn-primary" value="Create Hike" />
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      name: "",
      description: "",
      difficultyLevel: "",
      startAddress: "",
      endAddress: "",
      errors: [],
      status: "",
      user_id: localStorage.getItem("user_id")
    };
  },
  methods: {
    createHike: function() {
      var params = {
        name: this.name,
        description: this.description,
        difficulty_level: this.difficultyLevel,
        start_address: this.startAddress,
        end_address: this.endAddress,
        user_id: this.user_id
      };
      axios
        .post("/api/hikes", params)
        .then(response => {
          this.$router.push(`/hikes/${response.data.id}`);
        })
        .catch(error => {
          this.status = error.response.status;
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
