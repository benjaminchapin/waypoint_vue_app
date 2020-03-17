<template>
  <div class="container">
    <h2>{{ hike.name }}</h2>
    <p>Description: {{ hike.description }}</p>
    <p>Difficulty Level: {{ hike.difficulty_level }}</p>
    <br />
    <div v-for="waypoint in hike.waypoints">
      <p>Name: {{ waypoint.name }}</p>
      <p>Description: {{ waypoint.description }}</p>
      <p><img :src="waypoint.image_url" alt="" /></p>
    </div>
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
      waypoints: [],
      newWaypointDescription: "",
      newWaypointName: "",
      newWaypointImageURL: "",
      newWaypointAddress: ""
    };
  },
  created: function() {
    axios.get("/api/hikes/" + this.$route.params.id).then(response => {
      this.hike = response.data;
    });
  },
  methods: {
    createWaypoint: function() {
      var params = {
        name: this.newWaypointName,
        description: this.newWaypointDescription,
        image_url: this.newWaypointImageURL
      };
      axios.post("/api/waypoints", params).then(response => {
        this.waypoints.push(response.data);
        this.newWaypointImageURL = "";
        this.newWaypointName = "";
        this.newWaypointDescription = "";
        this.newWaypointAddress = "";
      });
    }
  }
};
</script>
