<template>
	<div class="">
		<button>Add GPS Point</button>
	</div>
	<div class="modal" v-if="errorStr">
		<slot></slot>
		Sorry, but the following error occurred: {{errorStr}}
		<button @click="$emit('close')">Dismiss</button>
	</div>

	<div class="modal" v-if="gettingLocation">
		<i>Getting your location...</i>
	</div>

	<div v-if="location">
		Your location data is {{ location.coords.latitude }}, {{ location.coords.longitude}}
	</div>
</template>

<script>
</script>

<style scoped>
::v-slotted(p) {
	font-style: italic;
}
</style>