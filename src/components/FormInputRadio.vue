<template>
    <label v-bind:for="input_name" class="">
        {{input_name}} {{is_required ? "*" : ""}}
    </label>
    <input ref="the_input" v-bind:type="input_type" v-bind:name="input_name" class="rounded-sm focus:ring-2 focus:ring-green-200" v-on:input="updateInput()" /> {{the_input}}
</template>
<script>
import { ref } from 'vue';
export default {
    props: {
        input_name: String,
        input_type: String,
        modelValue: String,
        is_required: Boolean,
    },
    emits: {

    },
    setup(props) {
        const the_input = ref('');
        const updateInput = () => {
            console.log(the_input.value.checked)
        }
        console.log(props);
        console.log(ref);

        return {
            the_input,
            updateInput,
        }
    }
};
</script>