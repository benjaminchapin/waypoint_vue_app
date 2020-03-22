<template>
  <div class="container">
    <h2>{{ hike.name }}</h2>
    <p>Description: {{ hike.description }}</p>
    <p>Difficulty Level: {{ hike.difficulty_level }}</p>
    <br />
    <div id="map"></div>
    <br />
    <form v-on:submit.prevent="updateHike()">
      <h1>Edit Hike</h1>
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        Name:
        <input v-model="hike.name" type="text" placeholder="name" />
        <br />
        Description:
        <input v-model="hike.description" type="text" placeholder="description" />
        <br />
        <div class="form-group">
          <label for="difficulty_levels">Difficulty Level:</label>
          <select id="difficulty_levels" type="text" class="form-control" v-model="hike.difficulty_level">
            <option value="novice">Novice</option>
            <option value="intermediate">Intermediate</option>
            <option value="advanced">Advanced</option>
          </select>
        </div>
        <br />
      </div>
      <button v-on:click="updateHike(hike)">Edit Hike</button>
    </form>
    <button v-on:click="destroyHike(hike)">Delete Hike</button>
    <br />
    <h1>Waypoints:</h1>
    <div v-for="waypoint in hike.waypoints">
      <p>Name: {{ waypoint.name }}</p>
      <p>Description: {{ waypoint.description }}</p>
      <p>Latitude: {{ waypoint.latitude }}</p>
      <p>Longitude: {{ waypoint.longitude }}</p>
      <p><img :src="waypoint.image_url" alt="" /></p>
      <br />
      <br />
      <form v-on:submit.prevent="updateWaypoint()">
        <h1>Edit Waypoint</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          Name:
          <input v-model="waypoint.name" type="text" placeholder="name" />
          <br />
          Description:
          <input v-model="waypoint.description" type="text" placeholder="description" />
          <br />
          Image URL:
          <input type="text" v-model="waypoint.image_url" placeholder="image URL" />
          <br />
          Address:
          <input type="text" v-model="waypoint.address" />
        </div>
        <button v-on:click="updateWaypoint(waypoint)">Edit Waypoint</button>
        <br />
        <button v-on:click="destroyWaypoint(waypoint)">Delete Waypoint</button>
      </form>
    </div>
    <p>{{ errors }}</p>
    <h1>Add Waypoint To Hike</h1>
    <div>
      Name:
      <input type="text" v-model="newWaypointName" />
      <br />
      Description:
      <input type="text" v-model="newWaypointDescription" />
      <br />
      <small v-if="newWaypointDescription" v-bind:class="{ 'text-danger': 100 - newWaypointDescription.length < 10 }">
        {{ 100 - newWaypointDescription.length }} characters remaining
      </small>
      <br />
      Image URL:
      <input type="text" v-model="newWaypointImageURL" />
      <br />
      Address:
      <input type="text" v-model="newWaypointAddress" />
      <br />
      <button v-on:click="createWaypoint()">Create Waypoint</button>
      <br />
    </div>
    <router-link to="/hikes">Back to all hikes</router-link>
  </div>
</template>

<style>
img {
  width: 150px;
}
#map {
  top: 0;
  bottom: 0;
  width: 100%;
  height: 600px;
}
</style>

<script>
/*global mapboxgl*/
import axios from "axios";

export default {
  data: function() {
    return {
      hike: {},
      newWaypointDescription: "",
      newWaypointName: "",
      newWaypointImageURL: "",
      newWaypointAddress: "",
      errors: []
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
        style: "mapbox://styles/mapbox/light-v10",
        center: [this.hike.start_long, this.hike.start_lat],
        zoom: 13
      });
      var startPopup = new mapboxgl.Popup({ offset: 25 }).setText(`${this.hike.start_lat},  ${this.hike.start_long}`);
      var endPopup = new mapboxgl.Popup({ offset: 25 }).setText(`${this.hike.end_lat},  ${this.hike.end_long}`);
      var start = new mapboxgl.Marker()
        .setLngLat([this.hike.start_long, this.hike.start_lat])
        .setPopup(startPopup)
        .addTo(map);
      var end = new mapboxgl.Marker()
        .setLngLat([this.hike.end_long, this.hike.end_lat])
        .setPopup(endPopup)
        .addTo(map);
      this.hike.waypoints.forEach(waypoint => {
        var popup = new mapboxgl.Popup({ offset: 25 }).setHTML(
          `<h2>${waypoint.name}</h2><img src="${waypoint.image_url}" alt="" />`
        );
        var marker = new mapboxgl.Marker()
          .setLngLat([waypoint.longitude, waypoint.latitude])
          .setPopup(popup)
          .addTo(map);
      });
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
          this.errors = error.response.data.errors;
        });
    },
    destroyWaypoint: function(waypoint) {
      axios.delete("/api/waypoints/" + waypoint.id).then(response => {
        var index = this.hike.waypoints.indexOf(waypoint);
        this.hike.waypoints.splice(index, 1);
        console.log("Waypoint deleted.", response.data);
      });
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
          // this.hike.waypoints.push(response.data);
          // this.$router.push(`/hikes/${this.hike.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    updateHike: function(hike) {
      var params = {
        name: hike.name,
        description: hike.description,
        difficulty_level: hike.difficulty_level
      };
      axios
        .patch("/api/hikes/" + hike.id, params)
        .then(response => {
          console.log("Hike updated!", response.data);
          // this.hike.waypoints.push(response.data);
          // this.$router.push(`/hikes/${this.hike.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyHike: function(hike) {
      axios
        .delete("/api/hikes/" + hike.id)
        .then(response => {
          this.$router.push("/hikes");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
