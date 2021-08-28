<template>
   <button @click="isOpen=!isOpen">Form stuff:{{isOpen}}</button>
   <div v-if="isOpen">

      <div class="fixed z-10 inset-0 overflow-y-auto" aria-labelledby="modal-title" role="dialog" aria-modal="true">
         <div class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">

         <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" aria-hidden="true"></div>
         <!-- This element is to trick the browser into centering the modal contents. -->
         <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>

         <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full">
            <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
              <div class="sm:flex sm:items-start">
                <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                  <h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-title">
                     
                     <button v-if="!hideMap" @click="hideMap=true">Hide Map</button>
                     <button v-if="hideMap" @click="hideMap=false">Show Map</button>

                     <!-- Hi rose -->

                     <!-- Map -->
                     <l-map v-if="!hideMap" ref="mapFormModal" :zoom="5" :center="[GPScoordinates.lat, GPScoordinates.lng]" style="z-index:15; height:20vh" dragging="false">
                        <l-geo-json ref="geojson" :geojson="geojson_data.features" :options="geojson_options"/>
                        <l-tile-layer
                           url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
                           layer-type="base"
                           name="OpenStreetMap"
                           :max-zoom="6"
                            />
                     </l-map>
                  </h3>

                  <!-- Data -->
                  <div class="mt-2">
                    <p class="text-sm text-gray-500">
                      ObjectId = {{}}
                    </p>
                  </div>
                </div>
              </div>
            </div>
            <div class="flex bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
              <button type="button" 
              class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-green-600 text-base font-medium text-white hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">
                Save
              </button>
               <button type="button" 
               @click="isOpen=!isOpen"
               class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-red-600 text-base font-medium text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 sm:ml-3 sm:w-auto sm:text-sm">
                Cancel
              </button>
            </div>
         </div>
      </div>
   </div>
   
   </div>
</template>

<script>
   import {ref, inject, onBeforeMount} from 'vue'
   import { LMap, LTileLayer, LGeoJson} from "@vue-leaflet/vue-leaflet";

   export default{

      components: {
         //leaflet
         LMap,
         LTileLayer,
         LGeoJson
       },

      setup() {
         const isOpen = inject('show_form_modal');
         const GPScoordinates = inject('GPScoordinates');
         const geojson_data = inject('geojson_data');
         const geojson_options = inject('geojson_options');
         const hideMap = ref(false);

         onBeforeMount( async ()=>{
            console.log('FormModal onBeforeMount');
        
              //style geojson
              const { circleMarker } = await import("leaflet/dist/leaflet-src.esm");
              geojson_options.pointToLayer = (feature, latLng) => circleMarker(latLng, { radius: 8 });
              
              //initialize map
              ref.mapIsReady = true;
         });

         return {
            isOpen, GPScoordinates, geojson_data, hideMap,

            LMap, LTileLayer, LGeoJson
         }
      }
   }
</script>