<template>
  <label for="checkbox">GeoJSON Visibility</label>
 
 <!-- Layer toggle -->

  <input
    id="checkbox"
    v-model="show"
    type="checkbox"
  >

<!-- Define map box -->

  <l-map ref="map" v-model:zoom="zoom" :center="[47.41322, -1.219482]" style="height:50vh">
    <l-geo-json v-model="geojson" v-if="show" :geojson="geojson" :options="geojsonOptions" />
    <l-tile-layer
      url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
      layer-type="base"
      name="OpenStreetMap"
      :max-zoom="10"
    />
    <l-marker :lat-lng="GPScoordinates" draggable @move="updateCoordinates"></l-marker>

  </l-map>
  <!-- Interactivity buttons -->
  <button @click="toggleModalState">Debug</button>
  <button @click="addGPSPoint()">Add GPS</button>
  <button @click="pushGPStoGeoJSON(), refreshGeoJSON()">Add to Layer</button>
  <modal 
    v-if="modalOpen"
    @close="toggleModalState"
  >
    <teleport to="#modal-wrapper"> 
      <p>A list of options</p>
      <p>Marker Coordinates: {{ GPScoordinates }}</p>
      <p>GeoJson: {{ geojson }}</p>
    </teleport>
  </modal>

</template>

<script>
// DON'T load Leaflet components here!
// Its CSS is needed though, if not imported elsewhere in your application.
import "leaflet/dist/leaflet.css"
import { LMap, LTileLayer, LGeoJson, LMarker } from "@vue-leaflet/vue-leaflet";
import Modal from "../components/Modal.vue";
// import AddGPSPoint from "../components/AddGPSPoint.vue";

export default {
  components: {
    Modal,
    LMap,
    LGeoJson,
    LTileLayer,
    LMarker,
  },
  data() {
    return {
      modalOpen: true,
      show: true,
      zoom: 2,
      geojson: {

        type: "FeatureCollection",

        features: [
          {
            "type" : "Feature", 
            "properties" : {  
              "dataType" : "lat lng coordinate",
              "notes"    : "test point." 
            }, 
            "geometry" : { 
              "type" : "Point", 
              "coordinates" : [15,30], 
            }
          }
        ],
      },

      geojsonOptions: {
        //can't use leaflet methods in here
        style: {
          "color": "#ff7800",
          "weight": 5,
          "opacity": 0.65
        },
      },

      GPScoordinates: null,

    };
  },
  watch: {
    geojson () {
      console.log('geojson update');
    }
  },
  methods: {
    toggleModalState() {
      this.modalOpen = !this.modalOpen;
    },
    updateCoordinates(e) {
      this.GPScoordinates = e.latlng;
    },
    addGPSPoint() {
      this.GPScoordinates = [50,50];
    },
    pushGPStoGeoJSON() {
      //add to geojson
      this.fixBigCoordinates().geojson.features.push(
        {
          "type" : "Feature", 
          "properties" : {  
            "dataType" : "lat lng coordinate", 
            "notes"    : "user data"
          }, 
          "geometry" : { 
            "type" : "Point", 
            "coordinates" : [this.GPScoordinates.lng, this.GPScoordinates.lat], 
          }
        }
      );
    },
    refreshGeoJSON() {
      // this.show = false;
      // this.show = true;
    },
    fixBigCoordinates() {
      if(this.GPScoordinates === null) {
        return this;
      }
      //keep within proper coordinate size
      if (this.GPScoordinates.lng > 180 || this.GPScoordinates.lng < -180) {
        console.log('feature out of bounds... fixing...');
        this.GPScoordinates.lng = (this.GPScoordinates.lng % 360 + 540) % 360 - 180;
      }

      if (this.GPScoordinates.lat > 90 || this.GPScoordinates.lat < -90) {
        console.log('feature out of bounds... fixing...');
        this.GPScoordinates.lat = (this.GPScoordinates.lat % 180 + 270) % 180 - 90;
      }

      return this;
    }
  },
  async beforeMount() {
    // HERE is where to load Leaflet components!
    const { circleMarker } = await import("leaflet/dist/leaflet-src.esm");

    // And now the Leaflet circleMarker function can be used by the options:
    this.geojsonOptions.pointToLayer = (feature, latLng) =>
      circleMarker(latLng, { radius: 8 });
    this.mapIsReady = true;
  },
  async updated() {
    console.log('DOM updated');
    // this.show = !this.show;
  }
};
</script>