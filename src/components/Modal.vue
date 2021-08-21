<template>
	<div class="modal">
		<slot></slot>
		<button @click="$emit('close')">Dismiss</button>
	</div>
</template>
<script type="text/javascript">
	export default {
	emits: [ "close" ]
	}
</script>
<style scoped>
::v-slotted(p) {
    font-style: italic;
  }
</style>