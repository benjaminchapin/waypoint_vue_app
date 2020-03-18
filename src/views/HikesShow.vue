<template>
  <div class="container">
    <h2>{{ hike.name }}</h2>
    <p>Description: {{ hike.description }}</p>
    <p>Difficulty Level: {{ hike.difficulty_level }}</p>
    <br />
    <div v-for="waypoint in hike.waypoints">
      <p>Name: {{ waypoint.name }}</p>
      <p>Description: {{ waypoint.description }}</p>
      <p>Latitude: {{ waypoint.latitude }}</p>
      <p>Longitude: {{ waypoint.longitude }}</p>
      <p><img :src="waypoint.image_url" alt="" /></p>
      <button v-on:click="destroyWaypoint(waypoint)">Delete Waypoint</button>
      <br />
      <br />
      <form v-on:submit.prevent="submit()">
        <h1>Edit Waypoint</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          Name:
          <input type="text" v-model="waypoint.name" />
          <br />
          Description:
          <input type="text" v-model="waypoint.description" />
          <br />
          Image URL:
          <input type="text" v-model="waypoint.image_url" />
          <br />
          Address:
          <input type="text" v-model="waypoint.address" />
        </div>
        <button v-on:click="updateWaypoint(waypoint)">Edit Waypoint</button>
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
  width: 250px;
}
</style>

<script>
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
      axios.delete("/api/waypoints/" + this.waypoint.id).then(response => {
        var index = this.hike.waypoints.indexOf(waypoint);
        this.hike.waypoints.splice(index, 1);
        console.log("Waypoint deleted.", response.data);
      });
    },
    updateWaypoint: function(waypoint) {
      var params = {
        name: this.waypoint.name,
        description: this.waypoint.description,
        image_url: this.waypoint.image_url,
        address: this.waypoint.address
      };
      axios.patch("/api/waypoints/" + this.waypoint.id, params).then(response => {
        this.hike.waypoints.push(response.data);
        console.log("Waypoint updated.", response.data);
      });
    }
  }
};
</script>
