<script lang="ts">
	import { isToneReady } from '$lib/stores/loadingStore';

	import { onMount } from 'svelte';
	import * as Tone from 'tone';

	export let hasStarted = false;

	let ambiancePlayer: Tone.Player;
	let ambiancePlayerLoaded = false;

	const setupAudio = () => {
		ambiancePlayer = new Tone.Player('ambient-lake.mp3', () => {
			ambiancePlayerLoaded = true;
		}).toDestination();
		ambiancePlayer.fadeIn = Tone.Time(5).valueOf();
		ambiancePlayer.volume.value = -12;
		ambiancePlayer.loop = true;
	};

	onMount(() => {
		Tone.loaded().then(setupAudio);
	});

	$: {
		if (ambiancePlayerLoaded) {
			console.log('ready');

			isToneReady.set(true);
		}
	}

	$: {
		if (hasStarted) {
			ambiancePlayer.start();
			console.log('playing: ambient audio');
		}
	}
</script>
