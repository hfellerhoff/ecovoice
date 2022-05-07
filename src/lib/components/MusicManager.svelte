<script lang="ts">
	import { sequences } from '$lib/sequences';

	import { isToneReady } from '$lib/stores/loadingStore';
	import { barnSwallowPlays, redWingedBlackbirdPlays } from '$lib/stores/musicStore';

	import { onMount } from 'svelte';
	import * as Tone from 'tone';

	export let hasStarted = false;

	let ambiancePlayer: Tone.Player;
	let ambiancePlayerLoaded = false;

	let synth: Tone.Sampler;

	const initSampler = (
		baseUrl,
		notes: string[],
		startOctave,
		endOctave
	): {
		urls: Record<string, string>;
		baseUrl: string;
	} => {
		const pianoNotes: string[] = [];

		for (let o = startOctave; o <= endOctave; o++) {
			notes.forEach((note) => pianoNotes.push(`${note}${o}`));
		}

		return {
			urls: pianoNotes.reduce((acc, cur) => {
				acc[cur] = `${cur}.ogg`;
				return acc;
			}, {}),
			baseUrl
		};
	};

	const setupAudio = () => {
		Tone.Transport.bpm.value = 80;
		Tone.Transport.start();

		synth = new Tone.Sampler({
			...initSampler('/piano/', ['C'], 2, 6),
			release: 1
		}).toDestination();

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
			isToneReady.set(true);
		}
	}

	$: {
		if (hasStarted) {
			ambiancePlayer.start();
			console.log('playing: ambient audio');
		}
	}

	const playSequence = (
		sequence: {
			time: string;
			pitch: string;
			dur: string;
		}[]
	) => {
		sequence.forEach((part) => {
			const [transportBars, transportQuarters, transportSixteenths] = Tone.Transport.position
				.toString()
				.split(':');
			const [partBars, partQuarters, partSixteenths] = part.time.toString().split(':');

			const time = `${Math.ceil(parseFloat(transportBars)) + Math.ceil(parseFloat(partBars))}:${
				Math.ceil(parseFloat(transportQuarters)) + Math.ceil(parseFloat(partQuarters))
			}:${Math.ceil(parseFloat(transportSixteenths)) + Math.ceil(parseFloat(partSixteenths))}`;

			synth.triggerAttackRelease(part.pitch, part.dur, time);
		});
	};

	$: {
		if (synth && $redWingedBlackbirdPlays > 0) {
			playSequence(sequences['red-winged-blackbird'].sequence);
		}
	}
	$: {
		if (synth && $barnSwallowPlays > 0) {
			playSequence(sequences['barn-swallow'].sequence);
		}
	}
</script>
