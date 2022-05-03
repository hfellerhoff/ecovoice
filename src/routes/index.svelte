<script>
	import Logo from '$lib/components/Logo.svelte';

	import BackgroundImage from '../lib/components/BackgroundImage.svelte';
	import FullHeight from '../lib/components/FullHeight.svelte';
	import Globe from '../lib/components/Globe.svelte';
</script>

<FullHeight>
	<Logo />
	<BackgroundImage src="bloom.jpg" alt="Zoomed out photo of a Lake Erie algal bloom" />
	<h1>Silent Killer in the Great Lakes</h1>
	<p>
		Since the 1960s, algal blooms in the Great Lakes have been growing and intensifying. Now, they
		threaten the stability of the Great Lakes ecosystems and the animals that live there.
	</p>
</FullHeight>

<style>
	p {
		max-width: 48rem;
		text-align: center;
	}
</style>
