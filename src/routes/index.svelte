<script lang="ts">
	import Logo from '$lib/components/Logo.svelte';
	import MusicManager from '$lib/components/MusicManager.svelte';
	import StartButton from '$lib/components/StartButton.svelte';

	import BackgroundImage from '../lib/components/BackgroundImage.svelte';
	import Introduction from '$lib/sections/Introduction.svelte';
	import MusicStart from '$lib/sections/MusicStart.svelte';

	import * as Tone from 'tone';

	let readyStates: Record<string, boolean> = {
		start: false,
		'music-start': false
	};

	const handleClick = (key: string) => () => {
		Tone.start();
		readyStates[key] = true;
	};
</script>

<MusicManager hasStarted={readyStates.start} />

<section>
	<Logo />
	<BackgroundImage src="bloom.jpg" alt="Zoomed out photo of a Lake Erie algal bloom" />
	<h1>Silent Killer in the Great Lakes</h1>
	<p>
		Since the 1960s, algal blooms in the Great Lakes have been growing and intensifying. Now, they
		threaten the stability of the Great Lakes ecosystems and the animals that live there.
	</p>
	<StartButton onClick={handleClick('start')} />
</section>

<div class:hide={!readyStates.start}>
	<Introduction hasStarted={readyStates.start} onContinue={handleClick('music-start')} />
</div>

<div class:hide={!readyStates['music-start']}>
	<MusicStart onContinue={handleClick('music-start')} />
</div>

<style>
	.hide {
		display: none;
	}

	p {
		max-width: 48rem;
		text-align: center;
	}
</style>
