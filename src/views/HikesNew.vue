<template>
  <div class="hikes-new">
    <div class="container">
      <form v-on:submit.prevent="submit()">
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
          <label>Difficulty Level:</label>
          <input type="text" class="form-control" v-model="difficultyLevel" />
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
      status: ""
    };
  },
  methods: {
    submit: function() {
      var formData = new FormData();
      formData.append("name", this.name);
      formData.append("description", this.description);
      formData.append("difficulty_level", this.difficultyLevel);
      formData.append("start_address", this.startAddress);
      formData.append("end_address", this.endAddress);
      axios
        .post("/api/hikes", formData)
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
