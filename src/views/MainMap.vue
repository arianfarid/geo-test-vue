<template>
    <!-- Form Modal -->

    <!-- Map Menu Items -->
    <div class="grid grid-rows-1 grid-flow-col justify-items-center bg-gray-100">
        <div class="flex space-x-1 m-0.5 ">
            <!-- Layers Dropdown -->
            <Dropdown>
                <template v-slot:toggler>
                    <button class="flex-none bg-gray-200 hover:bg-gray-100 p-2 mr-0.5 ml-0.5 mt-1 mb-1 shadow rounded-sm" @click="toggle">Toggle</button>
                </template>
                <DropdownContent>
                    <button>Example</button>
                </DropdownContent>
            </Dropdown>
            <!-- GeoJSON toggle -->
            <div class="flex-none bg-gray-200 hover:bg-gray-100 p-2 mr-0.5 ml-0.5 mt-1 mb-1 shadow rounded-sm">
                <label for="geojson_layer">GeoJson</label>
                <input id="geojson_layer" v-model="show_geoJson" class="appearance-none checked:bg-green-100" type="checkbox">
            </div>
            <!-- Basemap Visibility -->
            <div class="flex-none bg-gray-200 hover:bg-gray-100 p-2 mr-0.5 ml-0.5 mt-1 mb-1 shadow rounded-sm">
                <label for="checkbox_basemap">Basemap Visibility</label>
                <input id="checkbox_basemap" v-model="show_basemap" class="appearance-none checked:bg-green-100" type="checkbox">
            </div>
        </div>
    </div>

    <!-- Define map box -->
    <l-map ref="map" v-model:zoom="zoom" :center="[47.41322, -1.219482]" style="z-index:5; height:30vh">
        <l-geo-json ref="geojson" v-if="show_geoJson" :geojson="geojson_data" :options="geojson_options" />
        <l-tile-layer v-if="show_basemap" url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png" layer-type="base" name="OpenStreetMap" :max-zoom="10" />
        <l-marker :lat-lng="GPScoordinates" draggable @move="updateCoordinates"></l-marker>
    </l-map>
    <!-- Interactivity buttons -->

    <div class="grid grid-rows-1 grid-flow-col justify-items-center">
        <div class="flex flex-wrap m-0.5 ">
            <div class="flex-none bg-gray-200 hover:bg-gray-100 p-2 mr-0.5 ml-0.5 mb-1 mt-1 shadow rounded-sm">
                <button @click="addGPSPoint()">Add GPS</button>
            </div>
            <div class="flex-none bg-gray-200 hover:bg-gray-100 p-2 mr-0.5 ml-0.5 mb-1 mt-1 shadow rounded-sm">
                <button @click="toggleSlideUpForm()">Add to Layer</button>
            </div>
            <div class="flex-none bg-gray-200 hover:bg-gray-100 p-2 mr-0.5 ml-0.5 mb-1 mt-1 shadow rounded-sm">
                <button @click="toggleModalState">Debug</button>
            </div>
        </div>
    </div>
    <!-- Slide Up Form -->
    <slide-up-form v-if="show_slide_up_form" title="Add Point to "></slide-up-form>

    {{ geojson_data }}
</template>
<script>
import { ref, onBeforeMount, onMounted, provide, reactive, watch} from 'vue';
// import FormModal from "../components/FormModal.vue";
import Dropdown from "../components/Dropdown.vue";
import DropdownContent from "../components/DropdownContent.vue";
import SlideUpForm from "../components/SlideUpForm.vue";
import { LMap, LTileLayer, LGeoJson, LMarker } from "@vue-leaflet/vue-leaflet";
export default {

    components: {
        // FormModal,
        Dropdown,
        DropdownContent,
        SlideUpForm,
        //leaflet
        LMap,
        LTileLayer,
        LGeoJson,
        LMarker
    },
    setup() {

        const show_geoJson = ref(true);
        const show_basemap = ref(true);
        const zoom = ref(2);
        const GPScoordinates = ref(null);
        var geojson_data = ref({

            type: "FeatureCollection",

            features: [{
                "type": "Feature",
                "properties": {
                    "dataType": "lat lng coordinate",
                    "notes": "test point."
                },
                "geometry": {
                    "type": "Point",
                    "coordinates": [15, 30],
                }
            }],
        });
        var geojson_options = {
            //can't use leaflet methods in here
            style: {
                "color": "#34d399",
                "weight": 5,
                "opacity": 0.65
            },
        };

        const updateCoordinates = (e) => {
            GPScoordinates.value = e.latlng;
        };
        const addGPSPoint = () => {
            GPScoordinates.value = [50, 50];
        };
        const pushGPStoGeoJSON = () => {
            //add to geojson
            geojson_data.value.features.push({
                "type": "Feature",
                "properties": form_inputs,
                "geometry": {
                    "type": "Point",
                    "coordinates": [GPScoordinates.value.lng, GPScoordinates.value.lat],
                }
            });

            console.log("pushed data");
            show_geoJson.value = !show_geoJson.value;
        };
        provide('pushGPStoGeoJSON', pushGPStoGeoJSON);

        function fixBigCoordinates() {
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
        }


        //Form modal items
        const show_form_modal = ref(false);
        provide("show_form_modal", show_form_modal);
        provide("GPScoordinates", GPScoordinates);
        provide("geojson_data", geojson_data);
        provide("geojson_options");
        const openFormModal = () => {
            show_form_modal.value = !show_form_modal.value;
        };

        //slide up form items
        const show_slide_up_form = ref(false);
        const toggleSlideUpForm = () => {
            show_slide_up_form.value = !show_slide_up_form.value;
        }
        provide("toggleSlideUpForm", toggleSlideUpForm)
        const form_inputs = reactive([
          {
            "id": 1,
            "is_required": true,
            "input_name": "Tree Species",
            "input_type": "text",
            "form_value": '',
          },
          {
            "id": 2,
            "is_required": false,
            "input_name": "Date Planted",
            "input_type": "date",
            "form_value": '',
          },
          {
            "id": 3,
            "is_required": true,
            "input_name": "Alive",
            "input_type": "checkbox",
            "form_value": '',
          }
        ]);
        provide("form_inputs",form_inputs);

        const updateFormInputsData = (data) => {
          for (var i = 0; i < data.length; i++) {
            form_inputs[i].form_value = data[i];
          }
        }
        provide("updateFormInputsData", updateFormInputsData);


        onBeforeMount(async () => {
            console.log('onBeforeMount');

            //style geojson
            const { circleMarker } = await import("leaflet/dist/leaflet-src.esm");
            geojson_options.pointToLayer = (feature, latLng) => circleMarker(latLng, { radius: 8 });

            //initialize map
            ref.mapIsReady = true;
        })

        onMounted(async () => {
            console.log('onMounted');
            // ref.geojson.addData();
        })

        watch(geojson_data.value, async () => {
            console.log('watched geojson button add');
            const { circleMarker } = await import("leaflet/dist/leaflet-src.esm");
            geojson_options.pointToLayer = (feature, latLng) => circleMarker(latLng, { radius: 8 });

            show_geoJson.value = !show_geoJson.value;
        })

        return {
            //map properties
            zoom,
            GPScoordinates,
            show_geoJson,
            show_basemap,
            geojson_data,
            geojson_options,

            //methods
            openFormModal,
            updateCoordinates,
            addGPSPoint,
            pushGPStoGeoJSON,
            fixBigCoordinates,
            toggleSlideUpForm,
            updateFormInputsData,

            //modal
            show_form_modal,
            show_slide_up_form,

        }

    }
}
</script>