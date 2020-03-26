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
      <div class="row">
        <div class="col-sm-12">
          <!-- Main Page Content - Left -->
          <div id="main-page-content-left" class="section-container main-page-content clearfix">
            <div class="section-content clearfix">
              <h1 class="page_title">Create a hike</h1>

              <!-- Contact Details -->
              <h6>
                Add a hike to the hikes index...
              </h6>

              <div id="contact-form-container" class="clearfix">
                <ul>
                  <li class="text-danger" v-for="error in errors">{{ error }}</li>
                </ul>
                <form class="validate-form clearfix" v-on:submit.prevent="submit()">
                  <div class="form-group">
                    <label for="form-name">Name</label>
                    <input
                      type="text"
                      class="form-control validate-field required"
                      data-validation-type="string"
                      id="form-name"
                      v-model="name"
                      placeholder="Enter name"
                    />
                  </div>
                  <div class="form-group">
                    <label for="form-description">Description</label>
                    <input
                      type="text"
                      class="form-control validate-field required"
                      data-validation-type="string"
                      id="form-description"
                      v-model="description"
                      placeholder="Enter description"
                    />
                  </div>
                  <div class="form-group">
                    <label for="form-start-address">Start Address</label>
                    <input
                      type="text"
                      class="form-control validate-field required"
                      data-validation-type="string"
                      id="form-start-address"
                      v-model="startAddress"
                      placeholder="Enter start address"
                    />
                  </div>
                  <div class="form-group">
                    <label for="form-end-address">End Address</label>
                    <input
                      type="text"
                      class="form-control validate-field required"
                      data-validation-type="string"
                      id="form-end-address"
                      v-model="endAddress"
                      placeholder="Enter end address"
                    />
                  </div>
                  <div class="form-group">
                    <label for="form-difficulty-level">Difficulty Level</label>
                    <select id="form-difficulty_level" type="text" class="form-control" v-model="difficultyLevel">
                      <option value="novice">Novice</option>
                      <option value="intermediate">Intermediate</option>
                      <option value="advanced">Advanced</option>
                    </select>
                  </div>

                  <button type="submit" class="btn btn-outline-inverse btn-lg">Create Hike</button>
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
  </div>
</template>

<style></style>

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
    submit: function() {
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
