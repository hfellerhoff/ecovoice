<script lang="ts">
	import { isGlobeReady } from '$lib/stores/loadingStore';

	import createGlobe from 'cobe';

	export let visible: boolean;
	let hasUpdatedReadyStatus = false;

	$: {
		if (visible) {
			let phi = 0;
			let canvas = document.getElementById('cobe');

			const globe = createGlobe(canvas, {
				devicePixelRatio: 2,
				width: 1000,
				height: 1000,
				phi: 4,
				theta: 0,
				dark: 0,
				diffuse: 1.2,
				scale: 1,
				mapSamples: 16000,
				mapBrightness: 6,
				baseColor: [0.3, 0.3, 0.3],
				markerColor: [0.0980392157, 0.7843137255, 0.0862745098],
				glowColor: [1, 1, 1],
				offset: [0, 0],
				markers: [{ location: [41.8764416, -80.8928694], size: 0.05 }],
				onRender: (state) => {
					if (!hasUpdatedReadyStatus) {
						if (!$isGlobeReady) {
							isGlobeReady.set(true);
							hasUpdatedReadyStatus = true;
						}
					}

					// Called on every animation frame.
					// `state` will be an empty object, return updated params.
					state.phi = phi;
					phi += 0.0075;
				}
			});
		}
	}
</script>

<div>
	<canvas id="cobe" style="width: 500px; height: 500px;" width="750" height="750" />
</div>

<style>
	div {
		display: flex;
		align-items: center;
		justify-content: center;
	}
	canvas {
		z-index: 1;
	}
</style>
