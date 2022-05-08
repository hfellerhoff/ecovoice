<script lang="ts">
	import { sequences } from '$lib/sequences';

	import { isToneReady } from '$lib/stores/loadingStore';
	import {
		barnSwallowPlays,
		musicStage,
		northernWatersnakePlays,
		paintedTurtlePlays,
		redWingedBlackbirdPlays
	} from '$lib/stores/musicStore';

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
		synth.volume.value = -6;

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

	$: {
		if (synth && $northernWatersnakePlays > 0) {
			playSequence(sequences['northern-watersnake'].sequence);
		}
	}

	$: {
		if (synth && $paintedTurtlePlays > 0) {
			playSequence(sequences['painted-turtle'].sequence);
		}
	}

	let blackbirdTimeout: NodeJS.Timeout;
	let swallowTimeout: NodeJS.Timeout;
	let watersnakeTimeout: NodeJS.Timeout;
	let turtleTimeout: NodeJS.Timeout;
	let minimumPlaybackTime = 0;
	let maximumPlaybackTime = 0;

	function getRandomArbitrary(min: number, max: number) {
		return Math.random() * (max - min) + min;
	}

	const loopSequence = (
		name: string,
		sequence: {
			time: string;
			pitch: string;
			dur: string;
		}[]
	) => {
		const timeoutTime = getRandomArbitrary(minimumPlaybackTime, maximumPlaybackTime);

		if (name === 'red-winged-blackbird') {
			blackbirdTimeout = setTimeout(() => loopSequence(name, sequence), timeoutTime);
			playSequence(sequences['red-winged-blackbird'].sequence);
		}
		if (name === 'barn-swallow') {
			swallowTimeout = setTimeout(() => loopSequence(name, sequence), timeoutTime);
			playSequence(sequences['barn-swallow'].sequence);
		}
		if (name === 'northern-watersnake') {
			watersnakeTimeout = setTimeout(() => loopSequence(name, sequence), timeoutTime);
			playSequence(sequences['northern-watersnake'].sequence);
		}
		if (name === 'painted-turtle') {
			turtleTimeout = setTimeout(() => loopSequence(name, sequence), timeoutTime);
			playSequence(sequences['painted-turtle'].sequence);
		}
	};

	$: {
		if (blackbirdTimeout) clearTimeout(blackbirdTimeout);
		if (swallowTimeout) clearTimeout(swallowTimeout);
		if (watersnakeTimeout) clearTimeout(watersnakeTimeout);
		if (turtleTimeout) clearTimeout(turtleTimeout);

		if ($musicStage === 1) {
			minimumPlaybackTime = 1000;
			maximumPlaybackTime = 3000;
		}
		if ($musicStage === 2) {
			minimumPlaybackTime = 2000;
			maximumPlaybackTime = 4000;
		}
		if ($musicStage === 3) {
			minimumPlaybackTime = 3000;
			maximumPlaybackTime = 5000;
		}
		if ($musicStage === 4) {
			minimumPlaybackTime = 4000;
			maximumPlaybackTime = 6000;
		}
		if ($musicStage === 5) {
			minimumPlaybackTime = 5000;
			maximumPlaybackTime = 7000;
		}

		if ($musicStage !== 0 && $musicStage <= 5) {
			loopSequence('red-winged-blackbird', sequences['red-winged-blackbird'].sequence);
			loopSequence('barn-swallow', sequences['barn-swallow'].sequence);
			loopSequence('northern-watersnake', sequences['northern-watersnake'].sequence);
			loopSequence('painted-turtle', sequences['painted-turtle'].sequence);
		}
	}
</script>
