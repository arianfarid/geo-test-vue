<template>
  <label for="checkbox">GeoJSON Visibility</label>
  <input
    id="checkbox"
    v-model="show"
    type="checkbox"
  >

  <l-map ref="map" v-model:zoom="zoom" :center="[47.41322, -1.219482]" style="height:50vh">
    <l-geo-json v-model="geojson" v-if="show" :geojson="geojson" :options="geojsonOptions" />
    <l-tile-layer
      url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
      layer-type="base"
      name="OpenStreetMap"
      :max-zoom="10"
    />
    <l-tile-layer
      url="https://s3.amazonaws.com/te512.safecast.org/{z}/{x}/{y}.png"
      attribution="<a href='https://blog.safecast.org/about/'>SafeCast</a> (<a href='https://creativecommons.org/licenses/by-sa/3.0/'>CC-BY-SA</a>"
      :min-zoom="5"
      :max-zoom="7"
    />
    <l-marker :lat-lng="GPScoordinates" draggable @move="updateCoordinates"></l-marker>

  </l-map>
  <button @click="toggleModalState">Debug</button>
  <button @click="addGPSPoint()">Add GPS</button>
  <button @click="pushGPStoGeoJSON">Add to Layer</button>
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
    LMarker
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
      GPScoordinates: []
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
      this.geojson.features.push(
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
      this.geojsonOptions.pointToLayer;
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
  }
};
</script>