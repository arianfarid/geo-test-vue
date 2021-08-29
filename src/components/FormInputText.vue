<template>
    <label class="">
        {{input_name}}
    </label>
    <input type="text" name="" class="rounded-sm focus:ring-2 focus:ring-green-200">
</template>
<script>
export default {
    props: {
        input_name: String
    },
    setup() {
        return {}
    }
};
</script>