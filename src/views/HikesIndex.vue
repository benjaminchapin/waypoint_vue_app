<template>
  <div class="hikes-index">
    <!-- =========== Section 1: Top Banner =========== -->
    <div id="top-banner" class="container-fluid horizontal-section-container clearfix">
      <div class="row">
        <div class="col-xs-12">
          <img src="/images/temp/banner-4.jpg" alt="Green scenery" class="full-width-image" />
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
          <div id="main-page-content" class="section-container main-page-content clearfix">
            <div class="section-content">
              <h1 class="page_title">All Hikes</h1>
              <div class="row">
                <div class="col-sm-6">
                  <button v-on:click="currentUserId = $parent.getUserId()" class="btn btn-outline-inverse btn-sm">
                    My Hikes
                  </button>
                  <button v-on:click="currentUserId = ''" class="btn btn-outline-inverse btn-sm">
                    All Hikes
                  </button>
                </div>
                <div class="col-sm-6">
                  <select id="difficulty_levels" type="text" class="form-control" v-model="difficultyFilter">
                    <option value="">Sort by difficulty</option>
                    <option value="novice">Novice</option>
                    <option value="intermediate">Intermediate</option>
                    <option value="advanced">Advanced</option>
                  </select>
                </div>
              </div>

              <!-- Media List -->
              <div class="media-list clearfix">
                <!-- Media List Item -->
                <!-- filterBy(hikes, difficultyFilter, 'difficulty_level') -->
                <div
                  v-for="hike in filterBy(
                    filterBy(hikes, difficultyFilter, 'difficulty_level'),
                    currentUserId,
                    'user_id'
                  )"
                  class="media clearfix"
                  text-align:
                  center
                >
                  <!-- <div class="item w-icon col-sm-4"> -->
                  <!-- <div class="thumbnail item-content "> -->
                  <a href="#" class="pull-left img-mask-effect zoom scaleimage">
                    <img v-if="hike.image_url" :src="hike.image_url" alt="" />
                    <img v-else src="/images/temp/gallery/gallery-masonry-7.jpg" alt="Landscape, mountains" />
                    <i class="mask"><span class="glyphicon glyphicon-plus-sign medium"></span></i>
                  </a>
                  <div class="media-body">
                    <h4 class="media-heading">
                      <router-link :to="`/hikes/${hike.id}`">{{ hike.name }}</router-link>
                    </h4>
                    <div class="caption">
                      <p>{{ hike.description }}</p>
                    </div>
                    <p>
                      <router-link :to="`/hikes/${hike.id}`" class="btn btn-outline-inverse btn-sm">
                        View Hike
                      </router-link>
                    </p>
                  </div>
                  <!-- </div> -->
                  <!-- </div> -->
                  <!-- .media -->
                </div>
                <!-- .media-list -->
              </div>
            </div>
            <!-- .section-content -->
          </div>
          <!-- .section-container -->
          <!-- End: Main Page Content -->
        </div>
        <!-- .col-sm-12 -->
      </div>
      <!-- .row -->
    </div>
    <!-- .container-fluid -->
    <!-- End: Section 2 -->
  </div>
</template>

<style></style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      hikes: [],
      difficultyFilter: "",
      currentUserId: ""
    };
  },
  created: function() {
    axios.get("/api/hikes").then(response => {
      this.hikes = response.data;
      console.log(this.hikes);
    });
  },
  methods: {}
};
</script>
