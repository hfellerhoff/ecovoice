<script lang="ts">
	import PlaybackImage from '$lib/components/PlaybackImage.svelte';
	import {
		barnSwallowPlays,
		northernWatersnakePlays,
		paintedTurtlePlays,
		redWingedBlackbirdPlays
	} from '$lib/stores/musicStore';
	import * as Tone from 'tone';

	import { scrollRef } from 'svelte-scrolling';
	import ContinueButton from '../components/ContinueButton.svelte';

	export let onContinue: () => void;

	const handlePlayback = (bird: string) => () => {
		Tone.start();
		if (bird === 'red-winged-blackbird') redWingedBlackbirdPlays.set($redWingedBlackbirdPlays + 1);
		if (bird === 'barn-swallow') barnSwallowPlays.set($barnSwallowPlays + 1);
		if (bird === 'northern-watersnake') northernWatersnakePlays.set($northernWatersnakePlays + 1);
		if (bird === 'painted-turtle') paintedTurtlePlays.set($paintedTurtlePlays + 1);
	};
</script>

<section use:scrollRef={'music-start'}>
	<h2>A Lively Ecosystem</h2>
	<p>
		The western basin of Lake Erie is home to a variety of species, all of which contribute to
		create a thriving and lively ecosystem. Click or tap on the images below to take a listen to
		that ecosystem, with musical lines inspired from the animals' calls.
	</p>
	<div class="grid">
		<PlaybackImage
			src="red-winged-blackbird.jpg"
			alt="Red-winged blackbird"
			onClick={handlePlayback('red-winged-blackbird')}
		/>
		<PlaybackImage
			src="barn-swallow.jpg"
			alt="Barn swallow"
			onClick={handlePlayback('barn-swallow')}
		/>
		<PlaybackImage
			src="northern-watersnake.jpg"
			alt="Northern watersnake"
			onClick={handlePlayback('northern-watersnake')}
		/>
		<PlaybackImage
			src="painted-turtle.jpg"
			alt="Painted turtle"
			onClick={handlePlayback('painted-turtle')}
		/>
	</div>
	<ContinueButton scrollTo="stage-1" onClick={onContinue}>Continue</ContinueButton>
</section>

<style>
	.grid {
		display: grid;
		grid-template-columns: 1fr 1fr 1fr 1fr;
		margin: 0 8rem;
	}

	p {
		max-width: 48rem;
		text-align: center;
	}
</style>
