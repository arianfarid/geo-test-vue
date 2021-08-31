<template>
    <label v-bind:for="input_name" class="">
        {{input_name}} {{is_required ? "*" : ""}}
    </label>
    <input v-bind:type="input_type" @input="$emit('update:modelValue', $event.target.value)" v-bind:name="input_name" class="rounded-sm focus:ring-2 focus:ring-green-200">
</template>
<script>
export default {
    props: {
        input_name: String,
        input_type: String,
        is_required: Boolean,
        modelValue: String,
    },
    setup() {
        return {

        }
    }
};
</script>