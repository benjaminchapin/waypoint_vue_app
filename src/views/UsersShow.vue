<template>
  <div class="users-show">
    <!-- =========== Section 1: Top Banner =========== -->
    <div id="top-banner" class="container-fluid horizontal-section-container clearfix">
      <div class="row">
        <div class="col-xs-12">
          <img src="/images/temp/banner-2.jpg" alt="Green scenery" class="full-width-image" />
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
          <!-- Main Page Content -->
          <div class="section-content">
            <h2>{{ user.first_name }} {{ user.last_name }}</h2>
            <p>Email: {{ user.email }}</p>
            <p>Skill Level: {{ user.skill_level }}</p>
            <router-link v-bind:to="`/users/${user.id}/edit`" class="btn btn-outline-inverse btn-sm">
              Edit Account Information
            </router-link>
            <br />
            <br />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      user_id: localStorage.getItem("user_id"),
      user: {}
    };
  },
  created: function() {
    axios.get(`/api/users/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.user = response.data;
    });
  },
  methods: {}
};
</script>
