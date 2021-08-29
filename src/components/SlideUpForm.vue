<template>
    <div class="grid grid-rows-auto grid-cols-1 grid-flow-col justify-items-center bg-gray-100 rounded-t">
        <div class="grid space-x-1 m-0.5 grid-rows-auto grid-cols-1 text-left w-screen p-2">
            <div class="font-bold">
                <h3>{{title}} Active Layer</h3>
            </div>
            <div class="text-gray-400">
                <div>{{GPScoordinates.lat}}, {{GPScoordinates.lng}}</div>
            </div>

            <form-input-text input_name="Test"></form-input-text>
            
            <div class="flex flex-initial">
                <gray-button></gray-button>
            </div>
        </div>
    </div>
</template>
<script>
import { inject } from 'vue';
import GrayButton from "../components/GrayButton.vue";
import FormInputText from "../components/FormInputText.vue"
export default {
    components: {

        //form items
        FormInputText,
        GrayButton
    },
    props: {
        title: String,
    },
    setup() {
        const GPScoordinates = inject('GPScoordinates');

        return {
            GPScoordinates
        }
    }
}
</script>