<template>
   <button @click="isOpen=!isOpen">Form stuff:{{isOpen}}</button>
   <div v-if="isOpen">

      <l-map ref="map" v-model:zoom="zoom" :center="[GPScoordinates.lat, GPScoordinates.lng]" style="z-index:5; height:50vh">
         <l-geo-json ref="geojson" :geojson="geojson_data.features" :options="geojson_options" />
         <l-tile-layer
            v-if="show_basemap"
            url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
            layer-type="base"
            name="OpenStreetMap"
            :max-zoom="10"
          />
      </l-map>

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
                    Deactivate account
                  </h3>
                  <div class="mt-2">
                    <p class="text-sm text-gray-500">
                      Are you sure you want to deactivate your account? All of your data will be permanently removed. This action cannot be undone.
                    </p>
                  </div>
                </div>
              </div>
            </div>
            <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
              <button type="button" class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-red-600 text-base font-medium text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 sm:ml-3 sm:w-auto sm:text-sm">
                Deactivate
              </button>
              <button type="button" 
              @click="isOpen=!isOpen"
              class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">
                Cancel
              </button>
            </div>
         </div>
      </div>
   </div>

            <div >
                <div class="fixed pin z-50 overflow-auto bg-gray-200 flex">
                  <p class="">
                     GPS Coordinates: {{GPScoordinates}}
                  </p>
                  <p>
                     Ok: OK
                  </p>
               </div>
            </div>

      
         </div>
</template>

<script>
   import {inject} from 'vue'
   import { LMap, LTileLayer, LGeoJson} from "@vue-leaflet/vue-leaflet";

   export default{
      setup() {
         const isOpen = inject('show_form_modal');
         const GPScoordinates = inject('GPScoordinates');
         const geojson_data = inject('geojson_data')

         return {
            isOpen, GPScoordinates, geojson_data,

            LMap, LTileLayer, LGeoJson
         }
      }
   }
</script>