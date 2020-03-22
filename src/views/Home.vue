<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <br />
    <div id="map"></div>
  </div>
</template>

<style>
#map {
  top: 0;
  bottom: 0;
  width: 100%;
  height: 300px;
}
</style>

<script>
export default {
  data: function() {
    return {
      message: "Welcome to Waypoint !"
    };
  },
  mounted: function() {
    mapboxgl.accessToken =
      "pk.eyJ1IjoiemVuZGVzaWducyIsImEiOiJjazdheDM3bnAxOTAxM2VvZDdlOGxkbzFhIn0.RHXlcu2cNHjSE9UObfd0KA";
    var map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/light-v10",
      center: [-77.04, 38.907],
      zoom: 11.15
    });
  },
  created: function() {},
  methods: {}
};
</script>
