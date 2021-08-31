<template>
    <label v-bind:for="input_name" class="">
        {{input_name}} {{is_required ? "*" : ""}}
    </label>
    <input ref="the_input" v-bind:type="input_type" v-bind:name="input_name" class="rounded-sm focus:ring-2 focus:ring-green-200" v-on:input="updateInput()" @change="updateInput()" />
</template>
<script>
import { ref } from 'vue';
export default {
    props: {
        input_id: Number,
        input_name: String,
        input_type: String,
        modelValue: String,
        is_required: Boolean,
    },
    emits: ['formInputChange'],
    setup(props, context) {
        const the_input = ref('');
        const updateInput = () => {
            if (the_input.value.type === "checkbox") {
                console.log("checkbox");
                context.emit("formInputChange", { "id": props.input_id, "value": the_input.value.checked });
                // context.emit();
            } else if (the_input.value.type === "text") {
                console.log("text");
                context.emit("formInputChange", { "id": props.input_id, "value": the_input.value.value });

            } else if (the_input.value.type === "date") {
                console.log("date");
                context.emit("formInputChange", { "id": props.input_id, "value": the_input.value.value });

            }

        }



        return {
            the_input,
            updateInput,
        }
    }
};
</script>