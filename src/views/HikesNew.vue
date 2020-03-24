<template>
  <div class="hikes-new">
    <!-- =========== Section 1: Top Banner =========== -->
    <div id="top-banner" class="container-fluid horizontal-section-container clearfix">
      <div class="row">
        <div class="col-xs-12">
          <img src="/images/temp/banner-1.jpg" alt="Green scenery" class="full-width-image" />
        </div>
        <!-- .col-xs-12 -->
      </div>
      <!-- .row -->
    </div>
    <!-- End: Section 1: Top Banner -->

    <div class="container-fluid horizontal-section-container clearfix">
      <div class="container">
        <form v-on:submit.prevent="createHike()">
          <h1>New Hike</h1>
          <img v-if="status" v-bind:src="`https://http.cat/${status}`" />
          <ul>
            <li class="text-danger" v-for="error in errors">{{ error }}</li>
          </ul>
          <div class="form-group">
            <label>Name:</label>
            <br />
            <input type="text" class="form-control" v-model="name" />
          </div>
          <div class="form-group">
            <label>Description:</label>
            <br />
            <input type="text" class="form-control" v-model="description" />
          </div>

          <div class="form-group">
            <label for="difficulty_levels">Difficulty Level:</label>
            <br />
            <select id="difficulty_levels" type="text" class="form-control" v-model="difficultyLevel">
              <option value="novice">Novice</option>
              <option value="intermediate">Intermediate</option>
              <option value="advanced">Advanced</option>
            </select>
          </div>

          <div class="form-group">
            <label>Start Address:</label>
            <br />
            <input type="text" class="form-control" v-model="startAddress" />
          </div>
          <div class="form-group">
            <label>End Address:</label>
            <br />
            <input type="text" class="form-control" v-model="endAddress" />
          </div>
          <input type="submit" class="btn btn-primary" value="Create Hike" />
        </form>
      </div>
    </div>
  </div>
</template>

<style>
#difficulty_levels {
  width: 50%;
}
.form-control {
  padding: 14px 20px 12px 45px;
  width: 50%;
  text-align: center;
  display: inline-block;
}
</style>

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
