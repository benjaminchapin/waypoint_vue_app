<template>
  <div class="login">
    <!-- =========== Section 1: Top Banner =========== -->
    <div id="top-banner" class="container-fluid horizontal-section-container clearfix">
      <div class="row">
        <div class="col-xs-12">
          <img src="/images/temp/carousel-img3.jpg" alt="Green scenery" class="full-width-image" />
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
              <h1 class="page_title">Login</h1>

              <!-- Contact Details -->
              <h6>
                Login to your existing account...
              </h6>

              <div id="contact-form-container" class="clearfix">
                <ul>
                  <li class="text-danger" v-for="error in errors">{{ error }}</li>
                </ul>
                <form class="validate-form clearfix" v-on:submit.prevent="submit()">
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
      email: "",
      password: "",
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        email: this.email,
        password: this.password
      };
      axios
        .post("/api/sessions", params)
        .then(response => {
          axios.defaults.headers.common["Authorization"] = "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          localStorage.setItem("user_id", response.data.user_id);
          this.$router.push(`/users/${response.data.user_id}`);
        })
        .catch(error => {
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    }
  }
};
</script>
