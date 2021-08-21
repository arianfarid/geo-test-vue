<template>
  <l-map ref="map" v-model:zoom="zoom" :center="[47.41322, -1.219482]" style="height:50vh">
    <l-geo-json :geojson="geojson" :options="geojsonOptions" />
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
  </l-map>
  <button @click="toggleModalState">Options</button>

    <modal 
      v-if="modalOpen"
      @close="toggleModalState"
    >
      <teleport to="#modal-wrapper"> 
        <p>A list of options</p>
      </teleport>
    </modal>

</template>

<script>
// DON'T load Leaflet components here!
// Its CSS is needed though, if not imported elsewhere in your application.
import "leaflet/dist/leaflet.css"
import { LMap, LTileLayer, LGeoJson } from "@vue-leaflet/vue-leaflet";
import Modal from "../components/Modal.vue";

export default {
  components: {
    Modal,
    LMap,
    LGeoJson,
    LTileLayer
  },
  data() {
    return {
      modalOpen: true,
      zoom: 2,
      geojson: {
        type: "FeatureCollection",
        features: [
          // ...
        ],
      },
      geojsonOptions: {
        // Options that don't rely on Leaflet methods.
      },
    };
  },
  methods: {
    toggleModalState() {
      this.modalOpen = !this.modalOpen;
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
};
</script>