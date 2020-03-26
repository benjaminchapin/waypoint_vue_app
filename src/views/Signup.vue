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

    <!-- =========== Section 2 =========== -->
    <div class="container-fluid horizontal-section-container clearfix">
      <div class="row">
        <div class="col-sm-12">
          <!-- Main Page Content - Left -->
          <div id="main-page-content-left" class="section-container main-page-content clearfix">
            <div class="section-content clearfix">
              <h1 class="page_title">Signup</h1>

              <!-- Contact Details -->
              <h6>
                Signup for an account with us...
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
                      v-model="firstName"
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
                      v-model="lastName"
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
                      v-model="email"
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
                      v-model="address"
                      placeholder="Enter address"
                    />
                  </div>
                  <div class="form-group">
                    <label for="form-skill-level">Skill Level</label>
                    <select id="form-skill_level" type="text" class="form-control" v-model="skillLevel">
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
                      v-model="password"
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
                      v-model="passwordConfirmation"
                      placeholder="Enter password confirmation"
                    />
                  </div>
                  <button type="submit" class="btn btn-outline-inverse btn-lg">Submit</button>
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
