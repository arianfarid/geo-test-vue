<template>
	<div v-if="active">
		<slot/>
	</div>
</template>

<script>
	import { inject } from 'vue'
	export default {
		setup() {
			const sharedState = inject(sharedState);

		}

	}
</script>