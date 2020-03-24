<template>
  <div class="signup">
    <!-- =========== Section 1: Top Banner =========== -->
    <div id="top-banner" class="container-fluid horizontal-section-container clearfix">
      <div class="row">
        <div class="col-xs-12">
          <img src="/images/temp/carousel-img6.jpg" alt="Green scenery" class="full-width-image" />
        </div>
        <!-- .col-xs-12 -->
      </div>
      <!-- .row -->
    </div>
    <!-- End: Section 1: Top Banner -->

    <div class="container-fluid horizontal-section-container clearfix">
      <div class="container">
        <form v-on:submit.prevent="submit()">
          <h1>Signup</h1>
          <ul>
            <li class="text-danger" v-for="error in errors">{{ error }}</li>
          </ul>
          <div class="form-group">
            <label>First Name:</label>
            <br />
            <input type="text" class="form-control" v-model="firstName" />
          </div>
          <div class="form-group">
            <label>Last Name:</label>
            <br />
            <input type="text" class="form-control" v-model="lastName" />
          </div>
          <div class="form-group">
            <label>Location (Address):</label>
            <br />
            <input type="text" class="form-control" v-model="address" />
          </div>

          <div class="form-group">
            <label for="skill_levels">Difficulty Level:</label>
            <br />
            <select id="skill_levels" type="text" class="form-control" v-model="skillLevel">
              <option value="novice">Novice</option>
              <option value="intermediate">Intermediate</option>
              <option value="advanced">Advanced</option>
            </select>
          </div>

          <div class="form-group">
            <label>Email:</label>
            <br />
            <input type="email" class="form-control" v-model="email" />
          </div>
          <div class="form-group">
            <label>Password:</label>
            <br />
            <input type="password" class="form-control" v-model="password" />
          </div>
          <div class="form-group">
            <label>Confirm Password:</label>
            <br />
            <input type="password" class="form-control" v-model="passwordConfirmation" />
          </div>
          <input type="submit" class="btn btn-primary" value="Submit" />
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      firstName: "",
      lastName: "",
      address: "",
      skillLevel: "",
      email: "",
      password: "",
      passwordConfirmation: "",
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        first_name: this.firstName,
        last_name: this.lastName,
        address: this.address,
        skill_level: this.skillLevel,
        email: this.email,
        password: this.password,
        password_confirmation: this.passwordConfirmation
      };
      axios
        .post("/api/users", params)
        .then(response => {
          this.$router.push("/login");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
