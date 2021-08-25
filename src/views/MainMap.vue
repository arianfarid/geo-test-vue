<template>

  <!-- GeoJSON toggle -->
  <label for="checkbox">GeoJSON Visibility</label>
  <input
    id="checkbox"
    v-model="show"
    type="checkbox"
  >

  <!-- Basemap Visibility -->
  <label for="checkbox">Basemap Visibility</label>
  <input
    id="checkbox_basemap"
    v-model="show_basemap"
    type="checkbox"
  >

<!-- Define map box -->

  <l-map ref="map" v-model:zoom="zoom" :center="[47.41322, -1.219482]" style="height:50vh">
    <l-geo-json v-model="geojson" v-if="show_geoJson" :geojson="geojson" :options="geojsonOptions" />
    <l-tile-layer
      v-if="show_basemap"
      url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
      layer-type="base"
      name="OpenStreetMap"
      :max-zoom="10"
    />
    <l-marker :lat-lng="GPScoordinates" draggable @move="updateCoordinates"></l-marker>

  </l-map>


  <!-- Interactivity buttons -->

  <button @click="addGPSPoint()">Add GPS</button>
  <button @click="pushGPStoGeoJSON(), refreshGeoJSON()">Add to Layer</button>
  
  <button @click="toggleModalState">Debug</button>
  <modal 
    v-if="modalOpen"
    @close="toggleModalState"
  >
    <teleport to="#modal-wrapper"> 
      <p class="text-gray-500">A list of options</p>
      <p class="text-gray-500">Marker Coordinates: {{ GPScoordinates }}</p>
      <p class="text-gray-500">GeoJson: {{ geojson }}</p>
    </teleport>
  </modal>


</template>

<script>
  import {ref, onBeforeMount} from 'vue';
  import { LMap, LTileLayer, LGeoJson, LMarker} from "@vue-leaflet/vue-leaflet";
  export default {

    components: {
      LMap,
      LTileLayer,
      LGeoJson,
      LMarker
    },

    props: {

    },

    setup() {

      const modalOpen = ref(true);
      const show_geoJson = ref(true);
      const show_basemap = ref(true);
      const zoom = ref(2);
      const GPScoordinates = ref(null);
      var geojson =  {

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
      };
      var geojsonOptions = {
        //can't use leaflet methods in here
        style: {
          "color": "#ff7800",
          "weight": 5,
          "opacity": 0.65
        },
      };

      const toggleModalState = () => {
        modalOpen.value = !modalOpen.value;
      };
      const updateCoordinates = (e) => {
        GPScoordinates.value = e.latlng;
      };
      const addGPSPoint = () => {
        GPScoordinates.value = [50,50];
      };
      const pushGPStoGeoJSON = () => {
        // //add to geojson
        // fixBigCoordinates().geojson.features.push(
        //   {
        //     "type" : "Feature", 
        //     "properties" : {  
        //       "dataType" : "lat lng coordinate", 
        //       "notes"    : "user data"
        //     }, 
        //     "geometry" : { 
        //       "type" : "Point", 
        //       "coordinates" : [this.GPScoordinates.lng, this.GPScoordinates.lat], 
        //     }
        //   }
        // );
      };
      const refreshGeoJSON = () => {
        // this.show = false;
        // this.show = true;
      };
      const fixBigCoordinates = () => {
        // if(GPScoordinates === null) {
        //   return this;
        // }
        // //keep within proper coordinate size
        // if (this.GPScoordinates.lng > 180 || this.GPScoordinates.lng < -180) {
        //   console.log('feature out of bounds... fixing...');
        //   GPScoordinates.lng.value = (this.GPScoordinates.lng % 360 + 540) % 360 - 180;
        // }

        // if (this.GPScoordinates.lat > 90 || this.GPScoordinates.lat < -90) {
        //   console.log('feature out of bounds... fixing...');
        //   this.GPScoordinates.lat.value = (this.GPScoordinates.lat % 180 + 270) % 180 - 90;
        // }

        return this;
      };

      onBeforeMount( async ()=>{
        console.log('onBeforeMount');
        const { circleMarker } = await import("leaflet/dist/leaflet-src.esm");
        ref.geojsonOptions.pointToLayer = (feature, latLng) => circleMarker(latLng, { radius: 8 });
        ref.mapIsReady = true;
      })

      return {
        //map properties
        zoom, GPScoordinates, show_geoJson, show_basemap, geojson, geojsonOptions,
        //methods
        toggleModalState, updateCoordinates, addGPSPoint, pushGPStoGeoJSON, refreshGeoJSON, fixBigCoordinates
      }

    }
  }

  

</script>