<template>
<div>
  <div class='container'>
    <div class='map-content'>
      <HereMap id="map" ref="map" :center="center" />
    </div>
  </div>  
  <!-- <button @click.prevent="mapPotholeArray">map all potholes </button>  -->
        <!--  Commented this out because I just added the method to the mounted lifecycle hook--> 
</div>
</template>

<script>
import HereMap from '../components/HereMap'
import potholeService from '../services/PotholeService'

export default {
  name: 'map-module',
  components: {
    HereMap,
  },
  data() {
  return {
    // we are this as prop to the HereMap component 
  center:{ 
    lat: 40.42387869, 
    lng: -79.9779719
    },
    potholes: []
}
  
  },
  mounted() {
    potholeService.getAllPotholes().then((response) => {
        this.potholes = response.data;
        this.mapPotholeArray();
    });   
  },
  methods: {
    mapPotholeArray() {
      let map = this.$refs.map;

      for (let i = 0; i < this.potholes.length; i++) {
        let lat = this.potholes[i].lat;
        let lon = this.potholes[i].lon;
        let data = {};
        data.potholeId = this.potholes[i].potholeId;
        data.submitterId = this.potholes[i].submitterId;
        data.lat = this.potholes[i].lat;
        data.lon = this.potholes[i].lon;
        data.addr = this.potholes[i].addr;
        data.neighborhood = this.potholes[i].neighborhood;
        data.dateCreated = this.potholes[i].dateCreated;
        data.dateInspected = this.potholes[i].dateInspected;
        data.dateRepaired = this.potholes[i].dateRepaired;
        data.status = this.potholes[i].currentStatus;
        data.severity = this.potholes[i].severity;
        data.dimensions = this.potholes[i].dimensions;
        data.notes = this.potholes[i].notes;
        map.dropMarker({Latitude: lat, Longitude: lon}, data);
      }

    }

  }
}
</script>

<style>
#map {
  height: 690px;
  padding: 0;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.container {
  display: flex;
  justify-content: space-between;
  padding: 30px 0 30px 0;
}

.map-content {
  flex-grow: 3;
}
</style>