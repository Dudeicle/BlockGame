<template>
	<div id="app">
		<!-- <BlockGame /> -->
		<BlockGameNoCanvas />
	</div>
	
</template>

<script>
// import BlockGame from './components/BlockGame.vue'
import BlockGameNoCanvas from './components/BlockGameNoCanvas.vue'

export default {
	name: 'App',
	components: {
		// BlockGame,
		BlockGameNoCanvas
	}
}
</script>

<style>

</style>
