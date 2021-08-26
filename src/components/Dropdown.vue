<template>
	<div @click="toggle">
		<slot name="toggler">
			<button>Layers</button>
		</slot>
	</div>
</template>

<script>
	import { provide } from 'vue';
	export default {

		setup() {

			var sharedState = {
				active: false
			};

			const toggle = () => {
				console.log('Hi')
			}

			provide(sharedState, toggle);
			// const toggle = () => {
			// 	sharedState.value.active = !sharedState.value.active;
			// }
		}
	}


</script>