<template>
    <label v-bind:for="input_name" class="">
        {{input_name}} {{is_required ? "*" : ""}}
    </label>
    <input v-bind:type="input_type" v-bind:name="input_name" class="rounded-sm focus:ring-2 focus:ring-green-200"> {{the_input}}
</template>
<script>
export default {
    props: {
        input_name: String,
        input_type: String,
        modelValue: String,
        is_required: Boolean,
    },
    emits: {

    },
    setup() {
        const the_input = '';
        return {
            the_input,
        }
    }
};
</script>