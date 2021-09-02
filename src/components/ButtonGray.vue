<template>
	<button type="button" class="flex-none bg-gray-200 hover:bg-gray-100 p-2 mr-0.5 ml-0.5 mb-1 mt-1 shadow rounded-sm" >{{button_text}}</button>
</template>
<script>
export default {
	props: {
		button_text: String
	},
	setup() {

		return {}
	}
};
</script>