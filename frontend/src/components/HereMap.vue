<!-- find tutorial code here https://developer.here.com/tutorials/how-to-implement-a-web-map-using-vuejs/-->

<template>
  <div id="map" v-bind:style="this.$store.state.mapHeight">
  <!--In the following div the HERE Map will render-->
    <div id="mapContainer" style="height:100%;width:100%" ref="hereMap"></div>
    <!-- <button @click.prevent="getCoords">get coords</button>
    <button @click.prevent="reverseGeocode">reverseGeocode</button>
    <div>lat: {{this.lat}}, long: {{this.long}} </div> -->
  </div>

</template>

<script>
import neighborhoodService from "../services/NeighborhoodService";
import potholeService from "../services/PotholeService";

export default {
  name: "HereMap",
  props: {
    center: Object
    // center object lives on the MAP vue
  },
  data() {
    return {
      platform: {},
      map: {},
      markers: [],
      ui: {},
      lat: 0,
      long: 0,
      reverseGeocodeResponse: {},
      address: "",
      neighborhoodName: "",
      // You can get the API KEY from developer.here.com
    };
  },
  created() {
        // Initialize the platform object:
    this.platform = new window.H.service.Platform({
      apikey: "de4YVe5qMpVeuGpjsJ3AlPUe-mwokn-D-zmqGbx0JVg"
    });
  },
  async mounted() {
    this.initializeHereMap();
    this.getCoords();


  },
  methods: {
    initializeHereMap() { // rendering map

      const mapContainer = this.$refs.hereMap;
      const H = window.H;
      // Obtain the default map types from the platform object
      let maptypes = this.platform.createDefaultLayers();

      // update this.map (and display) a map object:
      this.map = new H.Map(mapContainer, maptypes.vector.normal.map, {
        zoom: 12,
        center: this.center
      });
      //add event listeners for reshaping the viewport and marker dragging
      addEventListener("resize", () => this.map.getViewPort().resize());
            

      // add behavior control
      new H.mapevents.Behavior(new H.mapevents.MapEvents(this.map));

      // add UI
      this.ui = H.ui.UI.createDefault(this.map, maptypes);
      // End rendering the initial map
      this.setUpClickListener(this.map);
    },
     getCoords() {
      //collect coordinates
      navigator.geolocation.getCurrentPosition((loc) => {
        this.lat = loc.coords.latitude;
        this.long = loc.coords.longitude;
      })  
    },
    reverseGeocode() {
      neighborhoodService.reverseGeocode(this.lat, this.long)
      .then((response => {
      this.reverseGeocodeResponse = response.data;
      this.neighborhoodName = response.data.results[0].address_components[2].long_name;
      const fullAddress = response.data.results[0].formatted_address;
      this.address = fullAddress.substring(0, fullAddress.length - 5);
      }))   
    },
    getAllPotholes() {
      potholeService.getAllPotholes().then((response) => {
        this.potholes = response.data;
        console.log(this.potholes)
      })
  },
    dropMarker(position, data) {
      const H = window.H;
      let marker = new H.map.Marker({lat: position.Latitude, lng: position.Longitude});
      //add listener to every marker
      marker.addEventListener("tap", () => {
        console.log(data)
        this.$store.state.currentMarker = data;
        });
    this.map.addObject(marker);
  },
    setUpClickListener(map) {
      map.addEventListener('tap', (evt) => {
      let coord = map.screenToGeo(evt.currentPointer.viewportX,
        evt.currentPointer.viewportY);
      console.log("Clicked at:" + coord.lat.toFixed(6) + " and " + coord.lng.toFixed(6));
      this.$store.commit("UPDATE_LAT", coord.lat.toFixed(6));
      this.$store.commit("UPDATE_LNG", coord.lng.toFixed(6));
      });
  }

   
}};
</script>

<style scoped>
#map {
  min-width: 360px;
  text-align: center;
  background-color: #ccc;
  margin: 0 auto;
}
</style>