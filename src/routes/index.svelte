<script>
	import Logo from '$lib/components/Logo.svelte';
	import MusicManager from '$lib/components/MusicManager.svelte';
	import StartButton from '$lib/components/StartButton.svelte';

	import BackgroundImage from '../lib/components/BackgroundImage.svelte';
	//onReady

	import Introduction from '$lib/sections/Introduction.svelte';

	let hasStarted = false;

	const onStart = () => {
		hasStarted = true;
	};
</script>

<MusicManager {hasStarted} />

<section>
	<Logo />
	<BackgroundImage src="bloom.jpg" alt="Zoomed out photo of a Lake Erie algal bloom" />
	<h1>Silent Killer in the Great Lakes</h1>
	<p>
		Since the 1960s, algal blooms in the Great Lakes have been growing and intensifying. Now, they
		threaten the stability of the Great Lakes ecosystems and the animals that live there.
	</p>
	<StartButton onClick={onStart} />
</section>

<div class:hide={!hasStarted}>
	<Introduction {hasStarted} />
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
