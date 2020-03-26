<template>
  <div class="users-edit">
    <!-- =========== Section 1: Top Banner =========== -->
    <div id="top-banner" class="container-fluid horizontal-section-container clearfix">
      <div class="row">
        <div class="col-xs-12">
          <img src="/images/temp/banner-6.jpg" alt="Green scenery" class="full-width-image" />
        </div>
        <!-- .col-xs-12 -->
      </div>
      <!-- .row -->
    </div>
    <!-- End: Section 1: Top Banner -->

    <!-- =========== Section 2 =========== -->
    <div class="container-fluid horizontal-section-container clearfix">
      <div class="row">
        <div class="col-sm-12">
          <!-- Main Page Content - Left -->
          <div id="main-page-content-left" class="section-container main-page-content clearfix">
            <div class="section-content clearfix">
              <h1 class="page_title">Edit User Information</h1>

              <!-- Contact Details -->
              <h6>
                Update your user information or delete your account...
              </h6>

              <div id="contact-form-container" class="clearfix">
                <ul>
                  <li class="text-danger" v-for="error in errors">{{ error }}</li>
                </ul>
                <form class="validate-form clearfix" v-on:submit.prevent="submit()">
                  <div class="form-group">
                    <label for="form-first-name">First Name</label>
                    <input
                      type="text"
                      class="form-control validate-field required"
                      data-validation-type="string"
                      id="form-first-name"
                      v-model="user.first_name"
                      placeholder="Enter first name"
                    />
                  </div>
                  <div class="form-group">
                    <label for="form-last-name">Last Name</label>
                    <input
                      type="text"
                      class="form-control validate-field required"
                      data-validation-type="string"
                      id="form-last-name"
                      v-model="user.last_name"
                      placeholder="Enter last name"
                    />
                  </div>
                  <div class="form-group">
                    <label for="form-email">Email address</label>
                    <input
                      type="email"
                      class="form-control validate-field required"
                      data-validation-type="email"
                      id="form-email"
                      v-model="user.email"
                      placeholder="Enter email"
                    />
                  </div>
                  <div class="form-group">
                    <label for="form-address">Address</label>
                    <input
                      type="text"
                      class="form-control validate-field required"
                      data-validation-type="string"
                      id="form-address"
                      v-model="user.address"
                      placeholder="Enter address"
                    />
                  </div>
                  <div class="form-group">
                    <label for="form-skill-level">Skill Level</label>
                    <select id="form-skill_level" type="text" class="form-control" v-model="user.skill_level">
                      <option value="novice">Novice</option>
                      <option value="intermediate">Intermediate</option>
                      <option value="advanced">Advanced</option>
                    </select>
                  </div>
                  <div class="form-group">
                    <label for="form-password">Password</label>
                    <input
                      type="password"
                      class="form-control validate-field required"
                      data-validation-type="string"
                      id="form-password"
                      v-model="user.password"
                      placeholder="Enter password"
                    />
                  </div>
                  <div class="form-group">
                    <label for="form-password-confirmation">Password Confirmation</label>
                    <input
                      type="password"
                      class="form-control validate-field required"
                      data-validation-type="string"
                      id="form-password-confirmation"
                      v-model="user.password_confirmation"
                      placeholder="Enter password confirmation"
                    />
                  </div>
                  <div class="pagination-container center clearfix">
                    <ul class="pagination-list">
                      <li class="float-left">
                        <button type="submit" class="btn btn-outline-inverse btn-lg">Submit</button>
                      </li>
                      <li class="float-right">
                        <button class="btn btn-outline-inverse" v-on:click="destroyUser()">Delete Account</button>
                      </li>
                    </ul>
                  </div>
                  <i class="form-loader fa fa-spinner fa-spin"></i>
                  <div class="form-group form-general-error-container"></div>
                </form>
              </div>
              <!-- #contact-form-container -->
            </div>
            <!-- .section-content -->
          </div>
          <!-- .section-container -->
          <!-- End: Main Page Content - Left -->
        </div>
        <!-- .col-sm-12   -->
      </div>
      <!-- .row -->
    </div>
    <!-- .container-fluid -->
    <!-- End: Section 2 -->
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      user: {},
      errors: []
    };
  },
  created: function() {
    axios.get(`/api/users/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.user = response.data;
    });
  },
  methods: {
    submit: function() {
      var params = {
        first_name: this.user.first_name,
        last_name: this.user.last_name,
        skill_level: this.user.skill_level,
        email: this.user.email,
        password: this.user.password,
        password_confirmation: this.user.password_confirmation
      };
      axios
        .patch(`/api/users/${this.user.id}`, params)
        .then(response => {
          this.$router.push(`/users/${this.user.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyUser: function() {
      confirm("Are you sure you want to delete your account?");
      axios
        .delete(`/api/users/${this.user.id}`)
        .then(response => {
          console.log("Account deleted.", response.data);
          this.$router.push("/home");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
      delete axios.defaults.headers.common["Authorization"];
      localStorage.removeItem("jwt");
      localStorage.removeItem("user_id");
      this.$router.push("/");
    }
  }
};
</script>
