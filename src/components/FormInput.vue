<template>
    <label class="">
        {{input_name}} {{is_required ? "*" : ""}}
    </label>
    <input v-bind:type="input_type" name="input_name" class="rounded-sm focus:ring-2 focus:ring-green-200">
</template>
<script>
export default {
    props: {
        input_name: String,
        input_type: String,
        is_required: Boolean,
    },
    setup() {
        return {}
    }
};
</script>