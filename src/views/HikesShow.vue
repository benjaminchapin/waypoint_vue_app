<template>
  <div class="hikes-show">
    <!-- =========== Section 2 =========== -->
    <div class="container-fluid horizontal-section-container clearfix">
      <div class="row">
        <div class="col-sm-12">
          <!-- Main Blog Items -->
          <div class="section-container main-page-content clearfix">
            <div class="section-content">
              <h2 class="page_title">
                <b>{{ hike.name }}</b>
              </h2>

              <!-- Blog List -->
              <div class="blog-articles-container full-width clearfix">
                <!-- Blog Single Article -->
                <article class="blog-article clearfix">
                  <div class="blog-article-content clearfix">
                    <p>
                      {{ hike.description }}
                    </p>

                    <div class="meta clearfix">
                      <div class="meta-item posted-by">
                        <span class="glyphicon glyphicon-user"></span>
                        Difficulty Level: {{ hike.difficulty_level }}
                      </div>
                      <div class="meta-item date">
                        <span class="glyphicon glyphicon-time"></span>
                        Created: {{ hike.created_at }}
                      </div>
                    </div>
                    <!-- .meta -->

                    <div id="map"></div>

                    <div class="text-content clearfix">
                      <div class="pagination-container center clearfix">
                        <ul class="pagination-list">
                          <li class="float-left">
                            <button
                              type="button"
                              class="btn btn-outline-inverse"
                              data-toggle="modal"
                              data-target="#updateHikeModal"
                            >
                              Edit Hike
                            </button>
                          </li>

                          <li class="float-right">
                            <button class="btn btn-outline-inverse" v-on:click="destroyHike()">
                              Delete Hike
                            </button>
                          </li>
                        </ul>
                      </div>
                      <!-- Modal -->
                      <div
                        class="modal fade"
                        id="updateHikeModal"
                        tabindex="-1"
                        role="dialog"
                        aria-labelledby="updateHikeModalLabel"
                        aria-hidden="true"
                      >
                        <div class="modal-dialog" role="document">
                          <div class="modal-content">
                            <div class="modal-body">
                              <h5 class="modal-title" id="updateHikeModalLabel">Edit Hike</h5>
                              <hr />

                              <form class="validate-form clearfix" v-on:submit.prevent="updateHike()">
                                <div class="form-group">
                                  <label for="form-name">Name</label>
                                  <input
                                    type="text"
                                    class="form-control validate-field required"
                                    data-validation-type="text"
                                    id="form-name"
                                    v-model="hike.name"
                                    placeholder="Enter name"
                                  />
                                </div>
                                <div class="form-group">
                                  <label for="form-description">Description</label>
                                  <input
                                    type="text"
                                    class="form-control validate-field required"
                                    data-validation-type="text"
                                    id="form-description"
                                    v-model="hike.description"
                                    placeholder="Enter description"
                                  />
                                </div>
                                <div class="form-group">
                                  <label for="form-difficulty-level">Difficulty Level</label>
                                  <br />
                                  <select
                                    id="difficulty_levels"
                                    type="text"
                                    class="form-control"
                                    v-model="hike.difficulty_level"
                                  >
                                    <option value="novice">Novice</option>
                                    <option value="intermediate">Intermediate</option>
                                    <option value="advanced">Advanced</option>
                                  </select>
                                </div>
                                <div class="pagination-container center clearfix">
                                  <ul class="pagination-list">
                                    <li class="float-left">
                                      <button type="submit" class="btn btn-outline-inverse btn-lg">Submit</button>
                                    </li>
                                    <li class="float-right">
                                      <button
                                        type="button"
                                        class="btn btn-outline-inverse btn-lg float-right"
                                        data-dismiss="modal"
                                      >
                                        Close
                                      </button>
                                    </li>
                                  </ul>
                                </div>

                                <i class="form-loader fa fa-spinner fa-spin"></i>
                              </form>
                            </div>
                          </div>
                        </div>
                      </div>
                      <!-- END MODAL -->
                    </div>
                    <!-- .text-content -->

                    <div id="comment-form" class="comment-box clearfix">
                      <h3>
                        <a
                          href="#"
                          id="toggle-comment-form"
                          onclick="$('#comment-form-container').slideToggle(); return false;"
                        >
                          <span class="glyphicon glyphicon-comment"></span>
                          Create a Waypoint
                        </a>
                      </h3>

                      <div id="comment-form-container" class="clearfix">
                        <form role="form">
                          <div class="form-group">
                            <label for="exampleInputName1">Name</label>
                            <input
                              type="text"
                              class="form-control"
                              id="exampleInputName1"
                              placeholder="Enter name"
                              v-model="newWaypointName"
                            />
                          </div>
                          <div class="form-group">
                            <label for="exampleInputEmail1">Description</label>
                            <input
                              type="text"
                              class="form-control"
                              id="exampleInputEmail1"
                              placeholder="Enter description"
                              v-model="newWaypointDescription"
                            />
                            <small
                              v-if="newWaypointDescription"
                              v-bind:class="{ 'text-danger': 100 - newWaypointDescription.length < 10 }"
                            >
                              {{ 100 - newWaypointDescription.length }} characters remaining
                            </small>
                          </div>
                          <div class="form-group">
                            <label for="exampleInputName1">Image URL</label>
                            <input
                              type="text"
                              class="form-control"
                              id="exampleInputName1"
                              placeholder="Enter Image URL"
                              v-model="newWaypointImageURL"
                            />
                          </div>
                          <div class="form-group">
                            <label for="exampleInputName1">Address</label>
                            <input
                              type="text"
                              class="form-control"
                              id="exampleInputName1"
                              placeholder="Enter Image URL"
                              v-model="newWaypointAddress"
                            />
                          </div>
                        </form>
                        <button type="submit" v-on:click="createWaypoint()" class="btn btn-outline-inverse">
                          Create Waypoint
                        </button>
                      </div>
                      <!-- #comment-form-container -->
                    </div>
                    <!-- .comment-box -->

                    <div id="comments-area" class="comment-box comments-list clearfix">
                      <h3>Waypoints</h3>

                      <!-- comment 1 -->
                      <div class="media clearfix " v-for="waypoint in hike.waypoints">
                        <a class="media-thumb">
                          <img class="media-object zoom" :src="waypoint.image_url" alt="Half moon" />
                        </a>
                        <div class="media-body">
                          <h4 class="media-heading">{{ waypoint.name }}</h4>
                          <p>{{ waypoint.description }}</p>
                        </div>
                        <div class="pagination-container center clearfix">
                          <ul class="pagination-list">
                            <li class="float-right">
                              <button
                                v-on:click="currentWaypoint = waypoint"
                                type="button"
                                class="btn btn-outline-inverse"
                                data-toggle="modal"
                                data-target="#updateWaypointModal"
                              >
                                Edit
                              </button>
                            </li>
                            <li class="float-right">
                              <button class="btn btn-outline-inverse" v-on:click="destroyWaypoint(waypoint)">
                                Delete
                              </button>
                            </li>
                          </ul>
                        </div>
                      </div>
                      <!-- .media -->
                      <!-- end: comment 1 -->
                    </div>
                    <!-- #comments-area -->
                  </div>
                  <!-- .blog-article-content -->
                </article>
                <!-- .blog-article -->
              </div>
              <!-- .blog-articles-container -->
              <!-- End: Blog List -->
            </div>
            <!-- .section-content -->
          </div>
          <!-- .section-container -->
          <!-- End: Main Blog Items -->
        </div>
        <!-- .col-sm-12 -->
      </div>
      <!-- .row -->
    </div>
    <!-- .container-fluid -->
    <!-- End: Section 2 -->

    <!-- Modal -->
    <div
      class="modal fade"
      id="updateWaypointModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="updateWaypointModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-body">
            <h5 class="modal-title" id="updateWaypointModalLabel">Edit Waypoint</h5>
            <hr />

            <form class="validate-form clearfix" v-on:submit.prevent="updateWaypoint(currentWaypoint)">
              <div class="form-group">
                <label for="form-name">Name:</label>
                <input
                  type="text"
                  class="form-control validate-field required"
                  data-validation-type="text"
                  id="form-name"
                  v-model="currentWaypoint.name"
                  placeholder="Enter name"
                />
              </div>
              <div class="form-group">
                <label for="form-description">Description:</label>
                <input
                  type="text"
                  class="form-control validate-field required"
                  data-validation-type="text"
                  id="form-description"
                  v-model="currentWaypoint.description"
                  placeholder="Enter description"
                />
              </div>
              <div class="form-group">
                <label for="form-image_url">Image Url:</label>
                <input
                  type="text"
                  class="form-control validate-field required"
                  data-validation-type="text"
                  id="form-image_url"
                  v-model="currentWaypoint.image_url"
                  placeholder="Enter image url"
                />
              </div>
              <div class="form-group">
                <label for="form-address">Address:</label>
                <input
                  type="text"
                  class="form-control validate-field required"
                  data-validation-type="text"
                  id="form-address"
                  v-model="currentWaypoint.address"
                  placeholder="Enter address"
                />
              </div>
              <div class="pagination-container center clearfix">
                <ul class="pagination-list">
                  <li class="float-left">
                    <button type="submit" class="btn btn-outline-inverse btn-lg">Submit</button>
                  </li>
                  <li class="float-right">
                    <button type="button" class="btn btn-outline-inverse btn-lg float-right" data-dismiss="modal">
                      Close
                    </button>
                  </li>
                </ul>
              </div>

              <i class="form-loader fa fa-spinner fa-spin"></i>
            </form>
          </div>
        </div>
      </div>
    </div>
    <!-- END MODAL -->
  </div>
</template>

<style>
#map {
  top: 0;
  bottom: 0;
  width: 100%;
  height: 600px;
}
.waypoint-marker,
.start-marker,
.end-marker {
  background-size: cover;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  cursor: pointer;
}

.waypoint-marker {
  background-image: url("https://cdn.iconscout.com/icon/premium/png-256-thumb/mushroom-1409567-1192660.png");
}

.start-marker {
  background-image: url("https://www.iconsdb.com/icons/preview/green/star-7-xxl.png");
}

.end-marker {
  background-image: url("https://cdn.iconscout.com/icon/premium/png-256-thumb/finish-flag-1763657-1503044.png");
}
</style>

<script>
/*global mapboxgl*/
import axios from "axios";
import dayjs from "dayjs";

export default {
  data: function() {
    return {
      hike: {},
      waypoint: {},
      waypoints: [],
      newWaypointDescription: "",
      newWaypointName: "",
      newWaypointImageURL: "",
      newWaypointAddress: "",
      updateHikeErrors: [],
      hikeDestroyErrors: [],
      updateWaypointErrors: [],
      createWaypointErrors: [],
      waypointDestroyErrors: [],
      currentWaypoint: {}
    };
  },
  created: function() {
    axios.get("/api/hikes/" + this.$route.params.id).then(response => {
      this.hike = response.data;
      console.log(response.data);
      mapboxgl.accessToken =
        "pk.eyJ1IjoiemVuZGVzaWducyIsImEiOiJjazdheDM3bnAxOTAxM2VvZDdlOGxkbzFhIn0.RHXlcu2cNHjSE9UObfd0KA";
      var map = new mapboxgl.Map({
        container: "map",
        style: "mapbox://styles/mapbox/outdoors-v11",
        center: [this.hike.start_long, this.hike.start_lat],
        zoom: 13
      });
      var startPopup = new mapboxgl.Popup({ offset: 25 }).setText(`${this.hike.start_lat},  ${this.hike.start_long}`);
      var endPopup = new mapboxgl.Popup({ offset: 25 }).setText(`${this.hike.end_lat},  ${this.hike.end_long}`);

      // create a DOM element for the marker
      var startEl = document.createElement("div");
      startEl.className = "start-marker";

      var start = new mapboxgl.Marker(startEl)
        .setLngLat([this.hike.start_long, this.hike.start_lat])
        .setPopup(startPopup)
        .addTo(map);

      // create a DOM element for the marker
      var endEl = document.createElement("div");
      endEl.className = "end-marker";

      var end = new mapboxgl.Marker(endEl)
        .setLngLat([this.hike.end_long, this.hike.end_lat])
        .setPopup(endPopup)
        .addTo(map);
      this.hike.waypoints.forEach(waypoint => {
        var popup = new mapboxgl.Popup({ offset: 25 }).setHTML(`<h2>${waypoint.name}</h2>`);

        // create a DOM element for the marker
        var el = document.createElement("div");
        el.className = "waypoint-marker";

        var marker = new mapboxgl.Marker(el)
          .setLngLat([waypoint.longitude, waypoint.latitude])
          .setPopup(popup)
          .addTo(map);
      });
      map.on(
        "load",
        function() {
          // helper function to push to coordinates array

          var coordinates = [[this.hike.start_long, this.hike.start_lat]];
          this.hike.waypoints.forEach(waypoint => {
            coordinates.push([waypoint.longitude, waypoint.latitude]);
          });
          coordinates.push([this.hike.end_long, this.hike.end_lat]);
          map.addSource("route", {
            type: "geojson",
            data: {
              type: "Feature",
              properties: {},
              geometry: {
                type: "LineString",
                coordinates: coordinates
              }
            }
          });
          map.addLayer({
            id: "route",
            type: "line",
            source: "route",
            layout: {
              "line-join": "round",
              "line-cap": "round"
            },
            paint: {
              "line-color": "#888",
              "line-width": 8
            }
          });
        }.bind(this)
      );
    });
  },
  methods: {
    createWaypoint: function() {
      var params = {
        name: this.newWaypointName,
        description: this.newWaypointDescription,
        image_url: this.newWaypointImageURL,
        address: this.newWaypointAddress,
        hike_id: this.hike.id
      };
      axios
        .post("/api/waypoints", params)
        .then(response => {
          console.log(response.data);
          this.hike.waypoints.push(response.data);
          this.newWaypointImageURL = "";
          this.newWaypointName = "";
          this.newWaypointDescription = "";
          this.newWaypointAddress = "";
        })
        .catch(error => {
          this.waypointCreateErrors = error.response.data.errors;
        });
    },
    destroyWaypoint: function(waypoint) {
      if (confirm("Are you sure you want to delete this waypoint?")) {
        axios
          .delete("/api/waypoints/" + waypoint.id)
          .then(response => {
            var index = this.hike.waypoints.indexOf(waypoint);
            this.hike.waypoints.splice(index, 1);
            console.log("Waypoint deleted.", response.data);
          })
          .catch(error => {
            this.waypointDestroyErrors = error.response.data.errors;
          });
      }
    },
    updateWaypoint: function(waypoint) {
      var params = {
        name: waypoint.name,
        description: waypoint.description,
        image_url: waypoint.image_url,
        address: waypoint.address
      };
      axios
        .patch("/api/waypoints/" + waypoint.id, params)
        .then(response => {
          console.log("Waypoint updated!", response.data);
          $("#updateWaypointModal").modal("hide");
        })
        .catch(error => {
          this.waypointUpdateErrors = error.response.data.errors;
        });
    },
    updateHike: function() {
      var params = {
        name: this.hike.name,
        description: this.hike.description,
        difficulty_level: this.hike.difficulty_level
      };
      axios
        .patch("/api/hikes/" + this.hike.id, params)
        .then(response => {
          console.log("Hike updated!", response.data);
          $("#updateHikeModal").modal("hide");
          // this.hike.waypoints.push(response.data);
          // this.$router.push(`/hikes/${this.hike.id}`);
        })
        .catch(error => {
          this.hikeUpdateErrors = error.response.data.errors;
        });
    },
    destroyHike: function() {
      if (confirm("Are you sure you want to delete this hike?")) {
        axios
          .delete("/api/hikes/" + this.hike.id)
          .then(response => {
            this.$router.push("/hikes");
          })
          .catch(error => {
            this.hikeDestroyErrors = error.response.data.errors;
          });
      }
    }
  }
};
</script>
