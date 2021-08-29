<template>
    <div class="grid grid-rows-auto grid-cols-1 grid-flow-col justify-items-center bg-gray-100 rounded-t">
        <div class="grid space-x-1 m-0.5 grid-rows-auto grid-cols-1 text-left w-screen p-2">
            <div class="font-bold">
                <h3>{{title}} Active Layer</h3>
            </div>
            <div class="text-gray-400 text-sm">
                <div>{{GPScoordinates.lat}}, {{GPScoordinates.lng}}</div>
            </div>
            <br>
            <form-input v-for="input in form_inputs" v-bind:key="input.id" v-bind:input_name="input.input_name" v-bind:input_type="input.input_type" :required="input.is_required" v-bind:is_required="input.is_required"></form-input>
            <div class="flex flex-initial">
                <button-gray></button-gray>
            </div>
            <div class="flex flex-initial">
                <button-save @click="pushGPStoGeoJSON(), toggleSlideUpForm()"></button-save>
            </div>
        </div>
    </div>
</template>
<script>
import { inject } from 'vue';
import ButtonSave from "../components/ButtonSave.vue"
import FormInput from "../components/FormInput.vue"
import ButtonGray from "../components/ButtonGray.vue";


export default {
    components: {

        //form items
        FormInput,
        ButtonGray,
        ButtonSave,
    },
    props: {
        title: String,
    },
    setup() {
        const GPScoordinates = inject('GPScoordinates');
        const form_inputs = inject('form_inputs');
        const pushGPStoGeoJSON = inject('pushGPStoGeoJSON');
        const toggleSlideUpForm = inject('toggleSlideUpForm');

        return {
            GPScoordinates,
            form_inputs,
            pushGPStoGeoJSON,
            toggleSlideUpForm,
        }
    }
}
</script>